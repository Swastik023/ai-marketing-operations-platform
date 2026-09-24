---
name: serp-tracker
description: "Deprecated pointer skill — SERP-feature tracking merged into rank-monitor, and this skill only explains the move and redirects you to run /omni-growth-engine:rank-monitor --features (AI Overviews, Featured Snippets, People Also Ask, Knowledge Panels, Local Pack, and more, plus feature ownership vs competitors). Triggers on \"/omni-growth-engine:serp-tracker\", \"track SERP features\", \"are we showing in AI Overviews\", \"who owns the featured snippet\", \"monitor the local pack\". Performs no tracking itself; pairs with /omni-growth-engine:rank-monitor for positions plus features, /omni-growth-engine:geo-monitor and /omni-growth-engine:aeo-audit for scored AI visibility, and /omni-growth-engine:gsc-ai-performance for real AI impressions."
---

# /omni-growth-engine:serp-tracker (deprecated → rank-monitor)

**This skill has been merged into `/omni-growth-engine:rank-monitor`.** SERP-feature tracking is now the `--features` mode of that single skill, so ranking positions and SERP features are captured in one run against one config.

## What to run instead

```
/omni-growth-engine:rank-monitor --features
```

That mode tracks the same features this skill used to — AI Overviews, Featured Snippets, People Also Ask, Knowledge Panels, Local Pack, Image Pack, Video Carousel, Shopping, Sitelinks — plus feature ownership vs. competitors, and the query-by-feature matrix.

## Notes on what changed

- **One skill, one config.** No more maintaining a separate SERP-feature keyword list — `rank-monitor` reads the same keyword set for both positions and features.
- **AI Overview tracking is a binary presence signal** (did an AI Overview appear, is the brand cited). It is **not** an AI-visibility score. For scored AI visibility across the 6 canonical surfaces use `/omni-growth-engine:geo-monitor` / `/omni-growth-engine:aeo-audit`; for actual AI impressions use `/omni-growth-engine:gsc-ai-performance`.
- **GSC does not export per-query SERP-feature layout or AI Overview citation lists** — feature presence comes from a connected rank-tracker MCP or manual observation. (An earlier version of this skill overstated GSC's coverage here.)

See `/omni-growth-engine:rank-monitor` for the full methodology, data-source notes, and output format.
