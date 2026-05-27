# Proof of Work

Tim Hieshima. I build and operate AI systems for go-to-market work.

I run Claude Code as my daily driver. Each of the systems below is something I built, operate, and use — not a demo, not a prototype.

## Case studies

| What | Scale | One line |
|---|---|---|
| **[CallScore](case-studies/callscore.md)** | 8+ sales calls scored, growing weekly | Audio in, sales scorecard out. Six-stage pipeline with a rubric I can tune. |
| **[X Bookmark Knowledge Graph](case-studies/bookmark-knowledge-graph.md)** | 1,962 bookmarks → synthesized graph | Daily backfill. Synthesis schema + index layer. Architecture for any lead-signal intelligence layer. |
| **[Brand Voice from 338k Words of Audio](case-studies/brand-voice-pipeline.md)** | 147 recordings, 338,082 words | Audio → analyzed → AI system prompt loaded as a Claude rule. Voice-grounded writing without generic-AI sound. |

## Other things I run

- **Multi-company agent orchestration** — I drive 8 separate companies (our agency, my AI education brand, my AI meetup, side projects) from one Paperclip control plane. Each has its own driver agent, scope rules, and notification policy. Live since early 2026.
- **CoS (Chief of Staff) infrastructure** — LaunchAgent layer that keeps the cross-agent truth surface (`AGENTS.md`) synced across Claude, Codex, and Hermes runtimes. Runs unattended.
- **AIxEDU Austin** — AI meetup I run at Capital Factory. 0 → 190 members in 5 months. Monthly events, speaker recruitment, content cadence.
- **Agency client work** — technical liaison for SMB clients. Strategy, automations, CRO audits.
- **ApplyAI** — public job-search tool at iteachyouai.com/tools/applyai.

## Stack I default to

- Claude Code for everything that touches text — code, briefs, content, ops
- Python for data + ops pipelines
- TypeScript / Next.js for anything user-facing
- Camoufox + Playwright for authenticated browser automation
- LaunchAgents (macOS launchd) for scheduled jobs
- SQLite for anything that doesn't need a server
- Vercel for hosting

## Contact

- Email: tim@iteachyouai.com
- LinkedIn: [in/timhieshima](https://www.linkedin.com/in/timhieshima/)
- GitHub: [tjp2021](https://github.com/tjp2021)
