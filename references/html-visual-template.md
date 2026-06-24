# HTML Visual Template — business-plan-harness

> In the final output stage, render the generated business-plan deliverables (.md) as a **self-contained HTML dashboard that is easy to read and centered on diagram visualization**.
> Goal: see **the structure of every deliverable at a glance**. Instead of copy-pasting the body text, visualize the structure and essentials with mind maps, Business Model Canvas, ecosystem maps, money-flow infographics, unit-economics charts, TAM/SAM/SOM, competition matrices, positioning scatters, funnels, journey maps, financial projections, cap tables, use-of-funds breakdowns, risk heatmaps, Gantt charts, KPI gauges, and webtoon panels (invent a topic-specific visualization when a standard diagram doesn't fit), and keep the details in collapsible sections. **Everything is inline SVG/CSS + generated images when possible — no external renderer (Mermaid etc.).** Because it's infographic-first, it is always included in the STEP 1-b default ("Both").

## When / What

- **Modes 1–4 (single document)**: one `<venture>-plan.html` file — section the document for that mode + visualizations matching the mode (only the applicable ones from the catalog below).
- **Mode 5 (full package)**: one `docs/index.html` file — a dashboard that synthesizes the 5 stages / 16 document types into the visualization catalog below. (Orchestrator: render right after the STEP 7 financial-assumption checkpoint, before STEP 8 delivery.)
- Render input = the already-PASSED final deliverables (.md). The HTML **must not generate new content** — only visualize the structure, numbers, and relationships of the existing deliverables (zero placeholders; numbers cited only from the deliverables and `00-business-model.md`).

## Technical policy (self-contained, one file, zero external dependencies)

- A single `.html`. CSS/JS/SVG **all inline**. **Zero external dependencies** — every visualization (mind maps, BMC, ecosystem maps, money-flow infographics, unit-economics charts, market-size diagrams, competition matrices, funnels, journey maps, financial projections, cap tables, risk heatmaps, Gantt charts, webtoons) is built with **hand-authored inline SVG/CSS**. **No external renderers/CDNs/plugins/web fonts such as Mermaid** (self-contained, offline, design control, Korean-safe).
  - Why not Mermaid: external CDN dependency (breaks offline / on intranets) + auto-layout means weak control over design and Korean labels and it turns sloppy. Hand-written SVG meets the infographic-first quality bar.
- **Generated images (when possible)**: for persona webtoons, ecosystem illustrations, concepts, etc., if you have image-generation capability, create them in `assets/` and embed via `<img>` (local relative path). Without that capability, fall back to inline SVG/CSS.
- A one-line comment at the top: "Fully self-contained (inline SVG/CSS) — works offline, no external dependencies."
- A restrained palette with one accent (e.g., indigo #4f46e5 + secondary teal). Dark text, light background. System fonts. No slop — at the level of a deliberately designed internal dashboard (whitespace, hierarchy, scannability).

## Visualization catalog (deliverable → visualization → technique)

> Technique = **all inline SVG/CSS** (no external renderers such as Mermaid). Webtoons/illustrations use generated images (`assets/`) when possible, otherwise CSS/SVG.

| # | Visualization | Source document | Technique (inline SVG/CSS) |
|---|--------|-----------|------------------------|
| 1 | **Overall structure mind map** (business → stage → document) | All / 01-business-plan | Inline SVG radial node-edge — placed in the hero, "whole structure at a glance" |
| 2 | **Value loop / core mechanism** (e.g., two-sided value cycle) | 01 / 04 | Inline SVG circular loop (arrows, labels) |
| 3 | **Ecosystem map** (participants, value network) | 03-competition-ecosystem | Inline SVG nodes (participants) + directed edges (value/money/data flow, labeled), own node highlighted |
| 4 | **Business Model Canvas** (9-block) | 00 / 01 | CSS grid 9 blocks (key partners·activities·resources·value props·relationships·channels·segments·costs·revenue) |
| 5 | **Money/revenue flow infographic** (Sankey-style) | 00-business-model | Inline SVG (split, pool distribution, amounts, Phase tags, Sankey-style) |
| 6 | **Unit economics chart** (CAC/LTV, breakdown, BEP) | 00 / 30 | Inline SVG bar/donut + LTV:CAC ratio gauge |
| 7 | **TAM/SAM/SOM market-size** | 02-market-analysis | Inline SVG concentric circles + value labels |
| 8 | **Segment / market breakdown** | 02 | Inline SVG donut/pie |
| 9 | **Competition matrix + positioning scatter** | 03 | CSS heatmap (✓/△/✗ color chips, own row highlighted) + inline SVG 2-axis scatter |
| 10 | **Persona webtoon panels** (situation → trigger → pain → resolution) | 04-customer-persona | Generated image cuts when possible, otherwise CSS comic cuts + speech/thought bubbles |
| 11 | **Persona journey map** (actions/emotions per stage) | 04 | Inline SVG horizontal timeline + emotion curve |
| 12 | **Value proposition canvas** (gains/pains/jobs ↔ relievers/creators) | 10-value-proposition | CSS two-panel VPC (customer profile ↔ value map) |
| 13 | **AARRR / acquisition funnel** (channels → conversion) | 20-gtm-strategy | Inline SVG funnel + channel-CAC matrix |
| 14 | **GTM timeline / phased roadmap** | 11 / 20 | Inline SVG horizontal timeline (Phase tags, milestones) |
| 15 | **Org chart + hiring plan** | 22-team-org | Inline SVG tree + hiring Gantt |
| 16 | **Financial projection** (3–5yr P&L / cash flow / runway) | 30-financial-model | Inline SVG multi-series line + area (runway), color per series |
| 17 | **BEP & sensitivity** | 30 | Inline SVG break-even graph + tornado sensitivity chart |
| 18 | **Use-of-funds + cap table** | 31-funding-plan | Inline SVG donut (use-of-funds) + stacked bar (cap table) |
| 19 | **Risk heatmap** (likelihood × impact) + kill-criteria | 32-risk-assessment | CSS grid heatmap + decision-tree SVG (kill-criteria) |
| 20 | **Milestone Gantt + KPI gauges + OKR tree** | 33-milestones-kpi | Inline SVG horizontal bar timeline + gauges + OKR tree |
| (topic-specific invented) | When a standard diagram doesn't fit the concept, invent a visualization just for that topic (quadrant, heatmap, ladder, etc.) | the relevant document | Inline SVG/CSS — creatively, fit to the domain |

> Modes 1–4 use only the applicable ones above (e.g., Mode 1 Lean → 1·2·4·5·6·7·9·13·16·20). Mode 5 uses all + topic-specific inventions. Every visualization is inline SVG/CSS (zero external dependencies).

## Layout

- Fixed left **sidebar TOC** (stage/document groups, jump) + main scroll (content max-width ~960px, line-height 1.7). Hamburger toggle on narrow screens (<900px).
- Sticky top bar: venture name + one-line summary + meta (date written, deliverable count) + `[🖨 Print/PDF]` (window.print).
- Hero: one-line value + stat cards (mode, deliverable count, MVP duration, one-line revenue model) + **overall structure mind map (#1)**.
- Per-stage sections: each section has its visualization + below it **collapsible (`<details>`) detailed essentials** (tables, lists, badges).
- **Badges**: priority P0 (red)·P1 (orange)·P2 (gray), Phase MVP (green)·Phase 2 (gray). ID tokens (`P0`·`Phase1`·`R3`·`T1`·`M2`) auto-styled as monospace chips. Gloss finance acronyms (ARPU/LTV/BEP/런웨이) in chip tooltips or captions.
- Interaction: sidebar active highlight (IntersectionObserver), smooth scroll, collapsible groups.

## Self-contained skeleton (fill the slots)

```html
<!doctype html><html lang="ko"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{TITLE}} — Business-plan dashboard</title>
<style>
/* variables: --accent, --accent2, --bg, --fg, --muted, --line; badge/chip/card/table/webtoon/print CSS */
/* layout: .layout{display:grid;grid-template-columns:260px 1fr} .sidebar{position:sticky;top:0;height:100vh;overflow:auto} */
/* tables: wrapper overflow-x:auto, zebra, sticky thead; pre: preserve monospace; .callout highlight box */
/* @media print{ .sidebar,.topbar,button{display:none} details{open} *{break-inside:avoid} main{width:100%} } */
/* @media (max-width:900px){ .layout{grid-template-columns:1fr} .sidebar{position:fixed;transform:translateX(-100%)} .sidebar.open{transform:none} } */
</style></head>
<body>
<header class="topbar">{{TITLE}} · {{ONELINE}} · {{META}} <button onclick="window.print()">🖨 Print/PDF</button></header>
<div class="layout">
  <nav class="sidebar" aria-label="TOC">{{TOC}}</nav>
  <main>
    <section id="hero">{{ONELINE}} {{STAT_CARDS}} <figure class="viz">{{MINDMAP_SVG}}</figure></section>
    {{MONEY_CALLOUT}}
    {{SECTIONS}}  <!-- per stage: visualization (inline <svg>/CSS, or <img src="assets/..">) + <details> detail -->
  </main>
</div>
<script>
  // inline SVG/CSS only — no external renderer/CDN (Mermaid not used).
  // IntersectionObserver active TOC highlight; hamburger toggle; beforeprint → details[open]=true
</script>
</body></html>
```
> Note: `lang="ko"` is a default; set it to match the chosen output language (deliverable language is user-selectable, default Korean — the HTML just renders whatever language the .md is in).

## Rules (check during audit)

- **Visualization is the lead**: fill the relevant catalog items **with real data** (no empty diagrams or placeholders). Financial/quantitative deliverables (00·30·31) are chart-dominant — money flow, unit economics, projections, BEP, cap table, and use-of-funds render as inline-SVG charts, not bare tables.
- **Content-faithful, no generation**: visualize only the deliverables' structure, numbers, and relationships. Money rules only cite `00-business-model.md` (no redefining). Don't invent numbers not in the deliverables (every number sourced or `ASSUMPTION:`-labeled).
- **Self-contained**: zero external dependencies — all visualizations inline SVG/CSS, **no Mermaid/CDN/web fonts/plugins**. Images are local in `assets/`. Opens on double-click, works fully offline.
- **Print**: hide sidebar/buttons, expand collapsibles, page-break-inside avoid, inline SVG prints as-is.
- **Accessibility**: semantic landmarks (nav/main/header), aria toggles, sufficient contrast, focus styles.
- **Plain language**: finance/business jargon glossed once in labels/captions (ARPU/LTV/BEP/런웨이). Calibrate to the broadest stakeholder (심사역·경영·비전문 투자자).
- **Korean**: tables/chips word-break safe, no overflow clipping.
- A deliberately designed dashboard — no generic Bootstrap dump.

## Reference note

For layout/composition reference, use any well-composed Mode-5 single-file dashboard (sidebar TOC + hero mind map + per-stage sections with collapsible details). **However, if any reference instance used a Mermaid CDN, that conflicts with the current policy** — new deliverables must be entirely inline SVG/CSS (zero external renderers). Reference only its layout/information structure, and replace the diagram technique with inline SVG.
