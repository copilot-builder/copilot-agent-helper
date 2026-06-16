# Microsoft Copilot Agent Optimizer

A single-file, browser-based tool that cleans up Microsoft 365 Copilot agent instructions: it trims filler the model already does by default, flags reference data that belongs in a knowledge file, rewrites vague phrasing, and helps you stay under Microsoft's **8,000-character** instructions limit.

Paste your agent's instructions, pick your Copilot subscription tier, and click **Analyze**. You get back a streamlined Markdown rewrite plus an optional separate knowledge file — with a part-by-part diagnosis explaining every change.

**Everything runs in your browser.** Nothing you paste is uploaded, sent to a server, or saved anywhere. There is no API key, no backend, and no AI call — the analysis is fully deterministic pattern matching.

<!-- Optional: add a screenshot once your repo is up
![Screenshot of the optimizer](docs/screenshot.png)
-->

> **Live demo:** https://copilot-builder.github.io/copilot-agent-optimizer/

---

## Why this exists

Copilot agent instructions have a hard 8,000-character cap, and a lot of what people put there is wasted: tone boilerplate ("be friendly and helpful"), persona openers ("you are an assistant designed to…"), polite softeners ("please remember to…"), and blocks of reference data (contact lists, acronym tables, policy URLs) that would work better as an attached knowledge file. All of that spends characters without changing how the agent behaves.

This tool finds those patterns, rewrites or removes them where it's safe to, and surfaces the rest as advisory flags for you to judge.

---

## Features

| | |
|---|---|
| **Streamlined rewrite** | Returns your instructions as a tightened Markdown bullet list, with a before → after character count and percent reduction. |
| **Knowledge file extraction** | Detects reference-shaped content (bulleted fact lists, data-dense paragraphs) and moves it into a separate Markdown knowledge file, grouped by topic. |
| **Tier-aware logic** | Basic plans can't attach knowledge files, so reference data is handled differently than on Premium / Copilot Studio (see [Subscription tiers](#subscription-tiers)). |
| **Part-by-part diagnosis** | Every part gets a verdict — Keep, Tighten, Delete, Move, or Mixed — with a plain-language reason. |
| **Advisory flags** | Judgment calls (vague conditionals, rationale clauses, no-fabrication rules) are flagged but left **unchanged** in the output. You decide. |
| **Live character budget** | A meter tracks your input against the 8,000-character limit as you type. |
| **Over-budget guidance** | If the cleaned output is still over 8K, it lists your longest remaining "keep" parts to review. |
| **Near-duplicate detection** | Flags parts with high word overlap so you can merge them. |
| **Negative-rule nudge** | If your instructions lean heavily on "don't / never / avoid," it suggests stating rules positively. |
| **Flattened-paste guard** | Warns when input arrives as one block with no line breaks (which it can't segment reliably). |
| **Copy & download** | Copy either output, or download as `agent-instructions.md` / `agent-knowledge.md`. |
| **Light / dark / auto theme** | Theme toggle, remembers your choice. |
| **Accessible** | ARIA live regions, screen-reader summaries, keyboard navigation, reduced-motion and high-contrast support. |
| **Zero dependencies** | One HTML file. No build step, no framework, no tracking. Works offline and from `file://`. |

---

## How it works

The tool runs a deterministic, rule-based pipeline. There is no machine learning and no network request — given the same input and tier, it always produces the same output.

1. **Segment.** Your text is split into *parts*: blank-line-separated blocks, with bulleted lists kept grouped (and a `Heading:` line directly above a list pulled in with it). Single-newline rules become their own parts.
2. **Classify.** Each part is matched against pattern sets and assigned a rubric category and an action:

   | Rubric category | Action | What it means |
   |---|---|---|
   | **Behavioral** | Keep | A genuine, non-default rule the agent needs to be told. |
   | **Vague** | Tighten | Wordy or unclear; the tool rewrites it. |
   | **Redundant** | Delete | Default model behavior (tone boilerplate, persona openers). |
   | **Knowledge** | Move | Reference data better suited to a knowledge file. |
   | *(mixed signals)* | Mixed | A part that needs more than one action; handled line by line. |

3. **Rewrite.** Tighten parts have their filler stripped (`please`, `remember to`, `in order to → to`, severity prefixes like `CRITICAL:`, etc.) and are reassembled as clean Markdown bullets. Delete parts are dropped. Move parts go to the knowledge file.
4. **Report.** You get the two outputs, a summary tally, and an expandable line-by-line breakdown with a reason for every verdict.

### What gets flagged

- **Tone / persona filler** — "you are a friendly, helpful assistant," "always be patient and warm," "your role is to…" → the model already does this, or the persona is filler. Purpose openers that bury a real mission ("you are an assistant *designed to help new hires onboard*") are salvaged into a direct statement rather than deleted.
- **Softeners and severity theater** — "please," "remember to," "make sure to," "try to," "it is important that you…," `CRITICAL:` / `IMPORTANT:` prefixes.
- **Vague conditionals** *(advisory)* — "if possible," "when appropriate," "as needed," "whenever possible."
- **Rationale clauses** *(advisory)* — "to ensure…," "so that users can…," "the reason is…" — explanations of *why*, not rules.
- **No-fabrication rules** *(advisory)* — "don't make things up" is usually default behavior, but reasonable to keep explicitly in high-stakes domains. Left to you.
- **Reference data → knowledge** — bulleted lists of contacts/URLs/codes, and long data-dense paragraphs.

---

## Subscription tiers

The tool asks which Microsoft Copilot subscription you have, because it changes the recommendation:

- **Basic** (Microsoft 365 Copilot Chat) — agents search the web only and **cannot** use knowledge files. Reference data can't be moved out, so the tool advises cutting it (if public, let the agent web-search) or condensing it (if private), and notes where Premium would help.
- **Premium or Copilot Studio** (Microsoft 365 Copilot / Microsoft Copilot Studio) — agents can use knowledge files, so reference data is extracted into a separate attachable Markdown file and stops counting against your 8,000 characters.

An in-app "Not sure which one you have?" panel compares all three tiers (Basic, Premium, Copilot Studio).

---

## Usage

### Run it locally

No install, no build. Just open the file:

```bash
# clone, then open the HTML in any modern browser
git clone https://github.com/copilot-builder/copilot-agent-optimizer.git
cd copilot-agent-optimizer
# open index.html in your browser
```

Or simply double-click the HTML file. It works fully offline.

### In the tool

1. Paste your agent's instructions into the box.
2. Pick your subscription tier.
3. Click **Analyze** (or **Load example** to see a sample run on a fictional "Contoso" onboarding agent).
4. Review the diagnosis, then **Copy** or **Download** the streamlined instructions and, on Premium/Studio, the knowledge file.
5. Replace your agent's Instructions field with the streamlined text; attach the knowledge file as a knowledge source.

---

## Deploy to GitHub Pages

This repo is already published with GitHub Pages, serving `index.html` at the site root:

**https://copilot-builder.github.io/copilot-agent-optimizer/**

To reproduce the setup on a fork: make sure the tool is named `index.html`, then go to **Settings → Pages → Build and deployment → Source: Deploy from a branch**, pick your default branch and the `/ (root)` folder, and save. Your site appears at `https://<your-username>.github.io/<your-repo>/` within a minute or two.

---

## Privacy

- No data leaves your browser. There is no server, no analytics, and no telemetry.
- The only external resource the page requests is a web font (Cascadia Code) from Google Fonts; the tool works without it.
- Your theme preference is stored in `localStorage` on your own device.

---

## Contributing

Issues and pull requests are welcome. Because the whole thing is one self-contained HTML file, contributing is straightforward: edit the file, open it in a browser, and test against your own agent instructions and the built-in example. When proposing changes to the classification rules, please describe the instruction pattern you're targeting and why the current behavior is wrong.

---

## Disclaimer

This is an independent, unofficial tool. It is **not affiliated with, endorsed by, or sponsored by Microsoft**. Microsoft, Copilot, and Copilot Studio are trademarks of Microsoft Corporation. The optimizer applies general guidance from Microsoft Learn but makes no guarantee that a given rewrite is correct for your use case — always review the output before saving it to a live agent.

Guidance references:
- [Configure high-quality instructions for generative orchestration](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/generative-mode-guidance)
- [Add knowledge sources to your declarative agent](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/knowledge-sources)
- [Write effective instructions for declarative agents](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/declarative-agent-instructions)
- [Build agents with Agent Builder in Microsoft 365 Copilot](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/agent-builder-build-agents)

---

## License

Released under the [MIT License](LICENSE).
