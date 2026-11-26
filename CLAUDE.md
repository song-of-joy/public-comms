# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Public-facing documents for **Gera Song of Joy CHS (Kharadi, Pune)** — a housing society. Remote: `song-of-joy/public-comms` on GitHub. It is not an application: there is no build, package manager, test suite or JavaScript. Every page is a single self-contained static HTML file that opens directly in a browser. Content is meant to be read by society residents and committee members, so wording, figures and legal/wage references matter more than code.

## Layout

- `index.html` — landing page; a grid of `a.card` links, one per document (tag, title, one-line summary, filename). **When adding, renaming or removing a document, update its card here**. Files have been renamed before (`2026-10-soj-housekeeping-stop.html` became `…-sop.html`), so check that the card's `href` and filename text still match.
- `YYYY-MM-<slug>.html` — one file per document, date-prefixed by the month it was issued (e.g. `2026-10-soj-security-sop.html`, `2026-10-security-gv-soj.html` = security agency bid comparison).
- `state-rules/` — official source PDFs (e.g. Maharashtra Lifts Rules 2026) that documents are drafted from. Document pages may link to them by relative path, as `2026-11-soj-lift-rules.html` does, so keep them committed and don't rename them without updating those links.
- Rules/legal summaries (e.g. the lift rules page) tie every duty to a citation chip (`.ref`: "Rule 10(13)", "Act s.36(1)", "Form ZI-8"). Non-legal advice is marked `.ref.own` ("Society practice"), and points needing human verification are marked `.check`. Keep that split when editing — never present a society practice as law.
- Public-facing notices (e.g. `2026-11-lift-users-notice.html`, the printable in-lift notice) carry no citations and may mix in society-practice items alongside the rules. Their footer naming the Rules stays as is. Don't flag or split those items there.

## Page conventions

The document pages were exported from Claude artifacts: each starts with a minified wrapper `<head>`/`<style>` block, followed by the page's own `<title>`, fonts and `<style>` inside `<body>`. Keep that structure when editing rather than "cleaning it up".

All pages share one visual system — keep new pages consistent with it:
- CSS custom properties on `:root`: `--bg --surface --fg --muted --line --accent --accent-soft`, status colours `--ok/--warn/--bad` (+ `-bg`), `--font-display` (Archivo) and `--font-body` (Source Sans 3, via Google Fonts).
- Dark mode via `@media (prefers-color-scheme:dark)` plus a `:root[data-theme="dark"|"light"]` override; define both palettes when adding colours.
- Safe-area-aware padding (`env(safe-area-inset-*)`), full-width reading layout, wide tables scroll inside their own frame.
- SOP pages use a sticky jump bar of section chips, with content grouped into cards and tables per section; the comment at the top of each page's `<style>` states its layout intent.
- No `<script>` anywhere — keep pages pure HTML/CSS. One exception: the jump-bar menu's one-line inline `onclick`, which only closes the menu after a section is tapped. It is optional, and the menu works without it.
- **Jump bar:** on desktop (900px and wider) it shows a row of section chips. Below 900px it becomes a "Sections ☰" menu: a hidden checkbox `#nav-toggle` plus a `label.nav-btn`, with the `ul` shown when the box is checked. Add new sections to that same `ul`. Every topic page (both SOPs, the bids comparison and the lift rules) has this bar. Its first item is always `<a class="home" href="index.html">← All documents</a>`. Give new topic pages the same bar.
- **No versioning.** Each page is always the current, final version. Don't add version numbers ("v2"), "what changed" sections, or "revised from…" notes. Edit the content in place instead. Git history is the record of changes.
- **Mobile (≤640px, screen only):** the SOP pages turn every `.tbl` table row into a card, and each `<td>` shows its column name from a `data-label` attribute (pulled from that table's `<thead>`). Any new or edited table row must carry `data-label` on its `<td>`s, or the cell loses its heading on phones. The row's leading `<th>` becomes the card title; empty `<td>`s are hidden. Housekeeping has extra rules: in the weekly `table.wk`, only the "●" days show, as chips; each column of the checklist `table.ck` becomes its own card. These rules are the last `<style>` block in each page. Desktop and print keep the normal table layout.

## Checking changes

There is no tooling. After editing, verify by re-reading the changed region, grepping for consistency across `index.html` and the document (filenames, dates like "Oct 2026", counts such as "8 agencies"), and opening the file in a browser if needed (ask before driving a headless browser).
