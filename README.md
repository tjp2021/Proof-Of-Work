# Proof of Work

Timothy Joo. I build and operate AI systems for go-to-market work.

I run Claude Code as my daily driver. Each of the systems below is something I built, operate, and use — not a demo, not a prototype.

## Case studies

| What | Scale | One line |
|---|---|---|
| **[CallScore](case-studies/callscore.md)** | 8+ sales calls scored, growing weekly | Audio in, sales scorecard out. Six-stage pipeline with a rubric I can tune. |
| **[X Bookmark Knowledge Graph](case-studies/bookmark-knowledge-graph.md)** | 1,962 bookmarks → synthesized graph | Daily backfill. Synthesis schema + index layer. Architecture for any lead-signal intelligence layer. |
| **[Brand Voice from 338k Words of Audio](case-studies/brand-voice-pipeline.md)** | 147 recordings, 338,082 words | Audio → analyzed → AI system prompt loaded as a Claude rule. Voice-grounded writing without generic-AI sound. *Code: [tjp2021/brand-voice](https://github.com/tjp2021/brand-voice)* |

## Other things I run

- **Multi-company agent orchestration** — I drive 8 separate companies (our agency, my AI education brand, my AI meetup, side projects) from one Paperclip control plane. Each has its own driver agent, scope rules, and notification policy. Live since early 2026.
- **[CoS (Chief of Staff) infrastructure](https://github.com/tjp2021/cos)** — governance layer for running Claude Code, Codex, and Copilot against one workspace: scheduled context regeneration, cross-tool rule sync, commit-time secret scanning, LLM-judge compliance evals. Runs unattended. Source is public.
- **AIxEDU Austin** — AI meetup I run at Capital Factory. 0 → 190 members in 5 months. Monthly events, speaker recruitment, content cadence.
- **Agency client work** — technical liaison for SMB clients. Strategy, automations, CRO audits.
- **ApplyAI** — AI resume-review tool. Shipped publicly on iteachyouai.com, retired April 2026.

## Stack I default to

- Claude Code for everything that touches text — code, briefs, content, ops
- Python for data + ops pipelines
- TypeScript / Next.js for anything user-facing
- Camoufox + Playwright for authenticated browser automation
- LaunchAgents (macOS launchd) for scheduled jobs
- SQLite for anything that doesn't need a server
- Vercel for hosting

## Contact

- LinkedIn: [in/timothyyjoo](https://www.linkedin.com/in/timothyyjoo/)
- GitHub: [tjp2021](https://github.com/tjp2021)
