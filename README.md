# Copilot Agent Optimizer

Diagnose and clean up Microsoft 365 Copilot agent instructions.

**[→ Try it live](https://copilot-builder.github.io/copilot-agent-optimizer/)**

## Why this exists

Microsoft 365 Copilot agents cap instruction text at 8,000 characters,
and instruction-following degrades well before you hit that limit — the
"curse of instructions" effect (Harada et al., 2024) shows compliance
decays exponentially as directive count grows. Most agent-builders
discover this the hard way: their 7,500-character instruction set
performs worse than a 1,500-character version of the same thing.

This tool helps you find what to cut.

## What it does

Paste your agent instructions, pick your tier (Basic, Premium, or
Copilot Studio), and get per-chunk verdicts: **delete**, **tighten**,
**move to knowledge base**, **split**, or **keep**. Plus a character
budget visualizer and an audit for common omissions
(tone, output format, agent-out path, self-evaluation, named tools).

Runs entirely in your browser. No data leaves your machine. No backend,
no API calls, no telemetry. One HTML file.

## How to use

1. Open the [live tool](https://copilot-builder.github.io/copilot-agent-optimizer/)
2. Paste your agent instructions
3. Select your Copilot tier
4. Review the verdicts and copy the cleaned output back into Agent Builder

## A note on knowledge-base offloading

Earlier versions of this tool suggested moving directive content to
knowledge sources to save instruction characters. **Don't do this.**
Microsoft's May 2026 guidance on cross-prompt injection (XPIA) classifiers
explicitly warns against treating knowledge files as instruction storage —
directives stored in knowledge content can be flagged as injection attempts.
The current version distinguishes between reference offloading (safe) and
directive offloading (not safe) and warns when it detects the latter.

## License

MIT — see [LICENSE](./LICENSE).

## Built by

Lance Daly. Find me on [LinkedIn](https://www.linkedin.com/in/YOUR-LINKEDIN-HANDLE).
Feedback, bug reports, and feature requests welcome via [Issues](../../issues).
