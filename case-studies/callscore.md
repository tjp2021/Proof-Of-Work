# CallScore — Audio-In, Scorecard-Out

A six-stage pipeline for scoring sales calls against a rubric. I use it on real calls from our agency work. The downstream dataset feeds an internal dashboard so reps can see their training log over time.

**Repo:** [github.com/tjp2021/callscore](https://github.com/tjp2021/callscore)
**Stack:** Python (uv-managed), Whisper for transcription, Claude for analysis, SQLite for the durable dataset, YAML for the rubric

## Why it exists

Sales managers want every call scored. Scoring is expensive: it takes ~30 min of human attention per call to write a useful scorecard, and most managers do it once and forget. The cost is enormous and the consistency is low.

We needed a pipeline that runs in minutes per call, produces output that's actually trustworthy enough to drive a training log, and uses a rubric we can tune without rewriting code.

## How it works

Six stages, each writes durable artifacts so a later stage can be re-run without losing earlier work:

1. **ingest** — accepts the recording, captures metadata
2. **preprocess** — normalizes audio, splits if long
3. **transcribe** — Whisper, with speaker-role assignment
4. **enrich** — computes audio-delivery metrics (pace, energy, talk ratio)
5. **analyze** — Claude scores the transcript against the rubric in `rubrics/og-sales.yaml`
6. **assemble** — builds the scorecard + appends to the training log

Each call lands in `data/callscore/<call-id>/` with its own folder. The `index.json` at the top is what downstream tools (our agency dashboard) read from.

CLI exposes `run`, `index`, `history`, `progress`, `doctor`, and `rescore`. The `rescore` flow is the one I use most — I tune the rubric, then re-run scoring on the existing transcripts without re-doing Whisper.

## What I learned

- **The rubric is the product.** The Whisper step and the Claude step are commodity; the rubric is what makes the output useful. I iterate on `og-sales.yaml` more than I iterate on the code.
- **Durable artifacts > clever pipelines.** Each stage writes a JSON blob. If something fails, I can re-run from any step. This pattern has saved me at least a dozen times.
- **Speaker-role assignment is the brittle part.** Whisper's diarization is good but not perfect. I had to write a custom role-assignment layer that uses transcript content + audio cues to decide who's the rep vs. the prospect.
- **The training log is what closes the loop.** Without the dashboard showing reps how their scores trend over time, the scoring is just notes nobody reads. Building the score is the easy half.

## GTM angle

This is the exact shape of what an AI Ops or GTM Engineer would build for a revenue team: structured AI analysis of an unstructured input (audio), durable output that downstream tools consume, and a rubric that the team can own and tune. I built it for our own agency and use it on real calls weekly.
