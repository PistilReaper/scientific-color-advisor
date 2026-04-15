---
name: scientific-color-advisor
description: Recommend scientific plotting palettes and PPT color systems with a local CLI backed by a vendored Scientific-Color-Lab template snapshot. Use when Codex needs palette advice for papers, line plots, scatter plots, bar charts, heatmaps, concept figures, course slides, or PPT decks, especially when the user wants export-ready HEX lists, Matplotlib or Plotly snippets, diagnostics, or safer alternatives to rainbow and red-green-heavy palettes.
license: Apache-2.0
---

# Scientific Color Advisor

## Overview

Use the local CLI at `scripts/scientific-color-cli.mjs` to turn scientific-figure or PPT requests into normalized protocol requests, ranked palette recommendations, diagnostics, and export-ready snippets. Prefer the vendored core so the skill works immediately after installation; use `--repo` or `SCIENTIFIC_COLOR_LAB_REPO` only when the user explicitly wants to point at a local `Scientific-Color-Lab` checkout.

## Workflow

1. Classify the request as either `scientific-figure` or `ppt`.
2. Normalize the request into the protocol described in [references/protocol.md](./references/protocol.md).
3. Call the CLI with explicit flags or `--request-json`.
4. Return the best recommendation first, then include exports or PPT usage notes as needed.
5. If the user wants reproducible downstream use, include the exact CLI command you used.

## Request Mapping

- Paper figures, chart palettes, line plots, scatter plots, bars, heatmaps, and concept figures map to `target=scientific-figure`.
- Slide themes, PPT chart colors, deck accents, and presentation color systems map to `target=ppt`.
- Require a compatible `chart_type` and `palette_class`. Reject unsupported combinations instead of inventing a fallback.
- Use `tone=restrained` for manuscript-safe or editorial requests, `tone=balanced` for neutral academic defaults, and `tone=strong` for poster or presentation-forward requests.
- Translate user concerns like "avoid rainbow", "avoid red/green", "grayscale safe", and "colorblind safe" into protocol priorities.

## CLI Usage

Recommendation:

```bash
node scripts/scientific-color-cli.mjs recommend --target scientific-figure --chart-type line-plot --palette-class qualitative --usage manuscript --background light --tone restrained
```

Export:

```bash
node scripts/scientific-color-cli.mjs export --target scientific-figure --chart-type heatmap --palette-class diverging --usage poster --background dark --tone strong --format matplotlib --format css
```

Health check:

```bash
node scripts/scientific-color-cli.mjs doctor --repo C:/path/to/Scientific-Color-Lab
```

## Output Guidance

- Keep the response concise and practical.
- Include the recommended template name, HEX list, and the reason it fits the requested scientific or PPT context.
- If `output=json` is useful for the user's workflow, mention that the CLI supports it.
- For PPT requests, include the slide-role pack and usage notes from [references/output-profiles.md](./references/output-profiles.md).

## Local Repo Mode

- Vendored mode is the default.
- If the user explicitly mentions a local `Scientific-Color-Lab` checkout, append `--repo <path>` or set `SCIENTIFIC_COLOR_LAB_REPO`.
- Use local repo mode for doctor or drift-check tasks first; do not assume the local checkout is always available.
