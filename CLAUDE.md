# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains a single standalone HTML widget: **Revenue Health Score Gauge** (`revenue_health_gauge_final.html`). It is a self-contained, zero-dependency component that renders a semi-circular SVG gauge displaying business revenue health metrics.

## Development

No build process, package manager, or toolchain is required. Open the file directly in a browser to develop and test.

To preview with custom values, open the file with URL parameters:
```
revenue_health_gauge_final.html?score=85&delta=-3
```

- `score` — integer 0–100 (default: 72)
- `delta` — integer points change vs. prior period, positive or negative (default: 5)

## Architecture

Everything lives in one file with three sections:

**CSS** — Inline `<style>` block. Layout is a centered flex card (max-width 220px). Key elements: `.gauge-wrap` (SVG container), `.score-row` (large number display), `.context-block` (delta + trajectory), `.band-pill` (status label).

**HTML** — Static shell with named element IDs (`gaugeSvg`, `scoreNum`, `deltaVal`, `trajIcon`, `trajText`, `bandPill`, `bandDot`, `bandText`) that JavaScript populates at runtime.

**JavaScript** — Inline `<script>` block at the bottom. Key functions:
- `scoreColor(s)` / `scoreBand(s)` / `scoreBandBg(s)` — map score to color, label, and background using four bands: Low (0–49, red), Moderate (50–69, orange), Good (70–84, green), Strong (85–100, cyan).
- `trajectoryText(s, d)` — combines current band + delta direction to produce a plain-English trajectory string and directional arrow icon.
- `buildGauge(score)` — renders the SVG programmatically: track arc, subtle zone-band fills, colored fill arc, boundary ticks at 0/50/70/85/100, and a needle-tip circle.

All rendering runs once on page load; there is no reactive state or event handling.

## Fonts

The widget loads DM Sans and DM Mono from Google Fonts via CDN. An internet connection is required for correct font rendering; the widget still functions without it but falls back to system sans-serif.
