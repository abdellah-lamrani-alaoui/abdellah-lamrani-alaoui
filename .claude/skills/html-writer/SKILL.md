---
name: html-writer
description: Write standalone, self-contained HTML reports/documents (analyses, summaries, status reports, comparisons, dashboards) in a warm editorial design system — ivory/clay/slate/olive palette, serif headings, mono data/labels. Use whenever the user asks to "summarize in html", "write this up as a report", "make a dashboard", or wants a polished single-file HTML deliverable instead of markdown/plain text.
---

# HTML Writer

Produce a single, self-contained `.html` file (inline `<style>`, no external assets, no build step, opens directly in a browser) that presents the requested content using the design system below. This is the same visual language used in Anthropic's `html-effectiveness` example gallery (https://github.com/thariqs/html-effectiveness) — a warm, editorial, slightly literary aesthetic that reads as considered rather than templated-AI-default.

Use this skill for: investment/financial analyses, research summaries, status/incident reports, comparison tables, PR write-ups, implementation plans, or any "turn this into an HTML doc" request. Don't use it for interactive web apps, multi-page sites, or anything requiring a backend.

## Design tokens

Always start from this CSS variable set (adjust accents only if the brief explicitly calls for different brand colors):

```css
:root {
  /* Neutrals */
  --ivory: #FAF9F5;
  --white: #FFFFFF;
  --slate: #141413;
  --gray-100: #F0EEE6;
  --gray-300: #D1CFC5;
  --gray-500: #87867F;
  --gray-700: #3D3D3A;

  /* Accents */
  --clay: #D97757;   /* primary accent, warning */
  --oat: #E3DACC;    /* highlight panel background */
  --olive: #788C5D;  /* success, low risk */
  --rust: #B04A3F;   /* danger, high risk */
  --info: #5C7CA3;   /* informational callouts */

  /* Type */
  --serif: ui-serif, Georgia, "Times New Roman", Times, serif;
  --sans: system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  --mono: ui-monospace, "SF Mono", Menlo, Monaco, Consolas, monospace;

  /* Spacing scale */
  --sp-1: 4px; --sp-2: 8px; --sp-3: 12px; --sp-4: 16px;
  --sp-5: 24px; --sp-6: 32px; --sp-7: 48px; --sp-8: 64px;

  /* Radius & elevation */
  --radius-panel: 12px;
  --radius-row: 8px;
  --radius-pill: 999px;
  --border: 1.5px solid var(--gray-300);
  --shadow-sm: 0 1px 2px rgba(20,20,19,0.06);
  --shadow-md: 0 4px 10px rgba(20,20,19,0.08);
  --shadow-lg: 0 12px 28px rgba(20,20,19,0.12);
}
```

Body background is `--ivory`, body text `--slate`, base font `--sans`. Page padding pattern: `56px 24px 120px` on body, content wrapped in a `.wrap { max-width: 1120px; margin: 0 auto; }` container (use 980px max-width for denser report-style documents).

## Typography rules

- **Headings and display numbers** (H1, H2, big stat figures): `--serif`, weight 500. H1 sizes responsively via `clamp(38px, 5vw, 62px)` on hero pages, or ~28px for report-style headers. H2 ~ 18-27px.
- **Body copy**: `--sans`, regular weight, line-height ~1.5.
- **Eyebrows, labels, data values, table numerics, status tags, code/PR references**: `--mono`, uppercase for labels with `letter-spacing: 0.04em–0.12em`, smaller size (11-13px).
- Never use more than these three font roles. Don't substitute generic sans-serif for headings — the serif/sans/mono split is the signature of this style.

## Core components

Build pages by composing these patterns (adapt content, keep the visual grammar):

1. **Hero / header band** — dark slate-to-deep-accent gradient or flat `--slate`/`--ivory` block, serif H1, sans subtitle, mono meta line (date, source) at reduced opacity. Rounded corners (`--radius-panel`), generous padding (`32-36px`).
2. **Banner / verdict callout** — white card, `1.5px` border, **left border 4-6px in the accent color** that signals the verdict (clay = primary pick, olive = positive/low-risk, rust = warning/avoid). Small uppercase mono "badge" pill above the heading.
3. **Stat cards** — grid (`repeat(auto-fit/auto-fill, minmax(220-316px, 1fr))`), white background, `1.5px` gray-300 border, `--radius-panel`, padding `18-22px`. Big serif numeral (26-44px) as the headline figure, mono uppercase label above/below, hover lifts (`translateY(-3px)`) with shadow and border color shifting to slate.
4. **Data tables** — `border-collapse: collapse`, header row background `--gray-100` with mono uppercase letter-spaced labels, numeric columns right- or tabular-aligned (`font-variant-numeric: tabular-nums`), row borders `1px solid var(--gray-300)`, wrap in a bordered `.table-scroll` div with `--radius-panel` for horizontal scroll on mobile.
5. **Badges / risk tags / status dots** — inline-flex pills (`--radius-pill`), small mono/sans text, colored background+text pairs: olive bg+text for low-risk/success, clay/amber bg+text for medium/warning, rust bg+text for high-risk/danger, a blue (`--info`) variant for neutral/informational.
6. **Callout boxes** — tinted background derived from the accent (e.g. clay at low opacity, oat solid, or info blue tint), matching border color, used for caveats, methodology notes, limitations — always state these plainly, this design system favors transparency over a glossy "everything is great" tone.
7. **Footer** — small mono/sans muted text, sources/citations, generation timestamp.

Use `--oat` as a panel background for "grouped" content (e.g. carryover items, supporting notes) rather than as a text color.

## Process

1. Identify the content's natural shape first (ranked list? comparison table? timeline? stat summary?) — pick components that fit the content, don't force every component into every doc. A short status update doesn't need a hero gradient; a single-finding report doesn't need a 4-card grid.
2. Build one self-contained `.html` file: inline `<style>` in `<head>`, no external fonts/CDNs/JS frameworks. System font stacks only — the whole point is zero dependencies and instant offline rendering.
3. Keep it responsive: at minimum collapse multi-column grids to one column under ~640-720px, and wrap wide tables in a horizontal-scroll container instead of letting them overflow the page.
4. Be honest in the content — if the brief includes caveats, uncertainty, or limitations (e.g. data quality, methodology gaps), surface them in a callout box, don't bury or omit them. This design system's "editorial" feel comes partly from that candor.
5. Reference `template.html` in this skill directory as a working starting point with all tokens and components pre-wired — copy and adapt it rather than starting from a blank file.

## Reference

- `template.html` — full working example with all components above, ready to duplicate and fill in.
- Design source: https://github.com/thariqs/html-effectiveness (gallery of 20 standalone HTML examples in this same system — status reports, incident reports, design-system references, flowcharts, etc. — fetch a specific numbered example via WebFetch on the raw GitHub URL if you need a pattern not covered here, e.g. `13-flowchart-diagram.html` for diagrams or `09-slide-deck.html` for slide-style layouts).
