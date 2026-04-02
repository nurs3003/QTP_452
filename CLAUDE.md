# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Quantitative Trading Model** built in R as a **Quarto document** (.qmd), submitted for FIN 452 (Financial Analytics and Trading). The project implements a clustering-based sector rotation strategy on S&P 500 Select Sector SPDR ETFs, using Kalman Filter signals.

**Due:** 2026-04-07 at 6pm Edmonton time. 10-minute in-class presentation. Worth 20% of grade. Individual assignment.

## Development Commands

All commands run from within an R session (RStudio or `R` in terminal):

```r
# Render the Quarto document to self-contained HTML
quarto::quarto_render("QTP_452.qmd", output_format = "html")

# Or from terminal:
# quarto render QTP_452.qmd --to html

# Reload packages without restarting R
devtools::load_all()

# Install/restore dependencies
renv::restore()   # if renv is used
```

## Project Structure

```
QTP_452.qmd        # main Quarto document — all analysis lives here
QTP_452.html       # self-contained rendered output (submitted)
context.md         # final context file (submitted alongside .qmd and .html)
CLAUDE.md          # this file
```

No local data files (CSV, Excel, RDS, etc.) are permitted. All data must be loaded via APIs at render time.

## Assets and Data

**Universe:** S&P 500 Select Sector SPDR ETFs + benchmark

| Ticker | Sector | Note |
|--------|--------|------|
| SPY | S&P 500 benchmark | |
| XLB | Materials | |
| XLC | Communication Services | Available from 2018 |
| XLE | Energy | |
| XLF | Financials | |
| XLI | Industrials | |
| XLK | Technology | |
| XLP | Consumer Staples | |
| XLRE | Real Estate | Available from 2015 |
| XLV | Health Care | |
| XLU | Utilities | |
| XLY | Consumer Discretionary | |

**Frequency:** Daily
**Training period:** June 2000 – December 2024
**Testing period:** January 2025 – March 2026 (inclusive)
**Rebalancing:** Monthly

## Strategy Requirements

- **Clustering** is the primary method for driving signals from the indicator mental model.
- **Kalman Filter** must be used in conjunction with clustering to trigger trade signals.
- Any additional indicators are allowed and encouraged to improve strategy quality.
- **Short selling is permitted.** The strategy is long-biased with a short overlay.

### Weight Allocation — Long/Flat/Short

| Signal | Condition | Weight |
|--------|-----------|--------|
| **Long** | Favored cluster (highest 6M trailing momentum) AND positive KF innovation | Proportional to innovation magnitude, normalized to **+100%** |
| **Short** | Unfavored cluster (lowest 6M trailing momentum) AND negative KF innovation | Proportional to \|innovation\|, normalized to **−30%** |
| **Flat** | All other sectors | 0% |

Net exposure: **+70%**. The strategy must demonstrate active management value above SPY.
- Decision rules must be clearly stated in the document and visualized using **Mermaid diagrams** (mermaid.ai). The code must match the mental model shown in the diagram.

## Workflow and Tooling

- All analysis must be in **R** inside the Quarto document. Python is not allowed — any Python code will be penalized.
- Follow the **tidymodels** workflow for model implementation (recipes, workflows, fit, predict).
- Use `tidyquant`, `quantmod`, or equivalent R packages to pull data via APIs.

## Submission Requirements

- Submit: `.qmd` file, self-contained `.html` file, and `context.md`.
- HTML must be **self-contained** (`embed-resources: true` in YAML header). A 5-point penalty applies if the prof has to re-render to see charts.
- Public **GitHub repo** must include all code. Share the repo link in the email submission.
- **Push commits daily** when working. A 5-point penalty applies for a single commit on the day of submission or insufficient commit history.

## Core Rules

- Be honest. Do not fabricate answers, results, confidence, or completion status.
- If something is uncertain, say so clearly.
- Be concise and direct.
- Do not default to agreement. Challenge weak assumptions and poor solutions.
- Speak up when the user is wrong — point out the error and explain why.
- Prefer simple, maintainable solutions over unnecessary complexity.
- Do not make major assumptions silently.
- Ask before making significant structural changes, broad refactors, or destructive edits.
- Do not claim code works unless it has been verified (rendered successfully).

## Communication

- Separate facts, assumptions, and recommendations.
- Explain tradeoffs when they matter.
- If a proposed approach is weak, say so directly and explain why.
- Ask clarifying questions when ambiguity would materially affect the result.

## Planning

- For non-trivial tasks, briefly restate the goal, identify open questions, and propose a plan.
- Prefer small, reviewable changes over large rewrites.
- Present alternatives only when there are genuinely viable choices.

## Implementation

- Prefer editing existing code before introducing new abstractions.
- Keep code readable, explicit, and easy to maintain.
- Use descriptive names for variables, chunks, and functions.
- Follow tidyverse/tidymodels conventions consistently throughout.
- Use `2-space indentation`.
- Avoid overengineering and premature optimization.
- Every code chunk should have a clear purpose visible from its label and output.

## Verification

- Validate work by rendering the full `.qmd` via `quarto::quarto_render()`.
- If full render is not possible, state what was checked and what was not.
- Confirm the HTML is self-contained before submission (`fs::file_size("QTP_452.html")` to sanity-check it embedded assets).
- Surface likely risks, edge cases, and failure points early (e.g. API rate limits, XLC/XLRE availability windows).

## Project Context

- Audience: FIN 452 instructor and peers. Prioritize clarity, explainability, and visual quality.
- The mental model (Mermaid diagram) and the code must be consistent — if the diagram says X triggers a trade, the code must implement exactly that.
- Optimize for readability, professional presentation, and ease of defending decisions in a 10-minute presentation.
