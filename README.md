# Copilot Agent Helper

Sorts your Microsoft Copilot agent's content into instructions and a knowledge file, and gives you a slimmer rewrite that stays under the 8,000-character instruction limit.

**Live tool:** https://copilot-builder.github.io/copilot-agent-helper/

## What it does

Microsoft 365 Copilot caps an agent's Instructions field at 8,000 characters. Paste your instructions in and the tool:

- Sorts every line into one of four categories: rules the agent needs (kept), facts it can look up (moved to a knowledge file), things it already does by default (removed), and vague phrasing (flagged to tighten).
- Returns a streamlined version of your instructions, ready to paste back in.
- Builds a separate knowledge file from the reference content, ready to download and attach.

It works for two setups:

- **Basic** (Copilot Chat): no knowledge file, so reference content stays in the instructions.
- **Premium or Copilot Studio**: reference content moves out to a knowledge file.

## How to use it

1. Open the live tool, or download `index.html` and open it in any browser.
2. Pick which Copilot you're building in.
3. Paste your agent's instructions, or click **Load example** to see it work.
4. Review the streamlined output and the knowledge file, then copy or download each.

## Privacy

Everything runs in your browser. Nothing you paste is uploaded or saved. Close the tab and it's gone.

## Notes

It's a single HTML file with no build step and no install. Download it and it runs offline.

Not affiliated with Microsoft. Microsoft, Copilot, and Copilot Studio are trademarks of Microsoft Corporation.
