# Brand Voice from 338k Words of Audio

A content analysis pipeline that takes raw audio (recordings, dictations, voice memos, podcast appearances) and produces an AI system prompt that reliably writes in my voice. The output is loaded as a Claude rule so any writing I do through Claude is grounded in measured stylistic data from my own corpus, not a generic "be conversational" instruction.

**Scale:** 147 audio recordings, 338,082 words analyzed
**Stack:** Whisper (local) for transcription, Python for the analysis pipeline, Claude for the synthesis layers, output ends as Markdown + a system-prompt fragment

## Why it exists

Every "AI writing tool" produces content that reads like the same warmed-over LinkedIn voice. The moment you use one for anything real — a sales email, a brand article, a teaching post — it sounds wrong. The voice isn't yours.

I wanted to fix this for my own writing first. I write under two brand identities (ITeachYouAI and personal/founder) and I burn time rewriting AI-generated drafts to sound like me. The pipeline turns that burn into a one-time investment: analyze enough of my actual spoken content, distill it into a system prompt, and Claude writes drafts that need 1-2 small edits instead of a full rewrite.

## How it works

The pipeline runs in 7 stages. Each writes durable Markdown + JSON so any stage can be re-run independently:

1. **Audio intake** — drops raw recordings (Apple Voice Memos, podcast exports, dictations) into a normalized format
2. **Local Whisper transcription** — runs locally because cloud transcription on 147 files is expensive and the privacy matters
3. **Per-file analysis** — sentence-length distribution, vocab frequency, filler-word stats, sentiment patterns
4. **Aggregation** — rolls per-file metrics into a corpus-level profile (`aggregated-metrics.json`)
5. **Theme + style extraction** — Claude reads the aggregated text and pulls recurring vocabulary, sentence patterns, what I avoid
6. **Profile assembly** — outputs `brand-voice-profile.md` (the full 9-section human-readable profile) and `system-prompt.md` (the version Claude actually reads)
7. **Integration** — system prompt gets dropped into Claude rules, used by content workflows

## What's in the output

Some specifics from my actual profile:
- Median sentence length: 7-10 words. Max 25.
- Paragraphs: 1-3 sentences. No walls of text.
- Strip ALL fillers in writing — no "like" as a hedge, no "right?", no "kind of," no "I think maybe"
- Vocabulary I do use: build, ship, love, cool, awesome, sick, crushing it
- Vocabulary I never use: leverage, synergy, stakeholder, circle back, excited to announce, game-changer
- "You" framing > "I" framing for teaching content

Every claim is grounded in word-frequency stats from the audio corpus. The system prompt cites the stats so Claude can self-correct when it drifts.

## Before / after — same prompt, same model

**Prompt:** *"Write a LinkedIn post announcing CallScore, the sales-call-scoring pipeline I built. Six stages: ingest, preprocess, transcribe, enrich, analyze, assemble. Audio in, scorecard out."*

**Without the brand voice rule (default Claude):**

> 🚀 Excited to announce CallScore — a powerful new pipeline I've built to revolutionize sales call analysis through AI.
>
> CallScore leverages a six-stage architecture (ingest, preprocess, transcribe, enrich, analyze, assemble) to deliver actionable scorecards from raw audio recordings. By bridging the gap between unstructured call data and structured insights, CallScore empowers sales leaders to drive consistent coaching outcomes at scale.
>
> Key benefits include:
> ✅ End-to-end automation
> ✅ Consistent, rubric-driven scoring
> ✅ Durable artifacts for downstream analytics
>
> I'm passionate about helping revenue teams unlock the full potential of their call data. Would love to connect with anyone working in sales enablement or RevOps!
>
> #AI #SalesEnablement #Innovation

**With the brand voice rule loaded:**

> Built a thing called CallScore.
>
> Audio in, sales scorecard out. Six stages: ingest, preprocess, transcribe, enrich, analyze, assemble. Each stage writes durable artifacts so I can re-run any step without losing earlier work.
>
> The rubric is a YAML file. That's the actual product. The Whisper step and the Claude step are commodity; the rubric is where the value lives. I tune it more often than I touch the code.
>
> Running it on real calls this week.

Same model. Same prompt. Same facts. The first one is what every "AI announcement post" looks like and nobody reads. The second one sounds like a person.

The difference isn't talent — it's that the second one is grounded in measured stylistic rules from the corpus: median sentence under 10 words, no buzzwords (leverage, empower, bridge, revolutionize), no excited-to-announce opener, no emoji, no hashtag bait, concrete claim instead of vague claim.

## What I learned

- **Generic AI writing is bad because the system prompt is generic.** Almost every "improve my writing" tool injects the same neutral middle-of-the-road style. The unlock is a system prompt grounded in actual measured behavior.
- **Audio is way easier than text as a source.** I speak more authentically than I write. Transcribing 30 minutes of voice gives me a better signal than 30 minutes of editing.
- **Frequency data beats opinions.** "I think I write conversationally" is useless. "Median sentence is 9 words, never uses 'leverage'" is actionable.
- **The system prompt has to live where the writing happens.** Mine is loaded as a Claude rule (path-scoped, auto-included), not pasted into prompts. If it's not always-on, you don't reach for it.

## GTM angle

This is the architecture every sales and marketing team is trying to build: a content engine that produces AI-drafted material that doesn't need a full rewrite to sound like the brand. The shape is the same — gather a corpus of real output, analyze it, distill it into a system prompt, deploy it where writing happens.

I built mine for personal voice. The same pipeline scales directly to:
- Sales: rep-specific cold-email voice that matches each AE
- Marketing: brand-voice profile derived from existing best-performing content
- CS: response-voice profile derived from top-performing CSM macros

The differentiator is that the profile is *measured*, not asserted.
