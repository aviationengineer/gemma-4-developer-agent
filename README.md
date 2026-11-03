# Gemma 4 Developer Agent Competition

Kaggle competition · **$110,000** ($65K main + $35K paper track + $10K small-model bonus)

> Post-train an open Gemma 4 model into an **offline coding agent that fixes
> hidden Python bugs**. Free Kaggle GPU notebooks.

- Competition: https://www.kaggle.com/competitions/gemma-4-developer-agent/overview

## ⚠️ URGENT gates

| Gate | Date |
|---|---|
| Accept competition rules (entry gate) | **Nov 25, 2026** |
| Paper track submission | **Nov 12, 2026** |
| Main deadline | **Dec 2, 2026** |

You cannot submit without accepting the rules — do this first.

## Roadmap

| Phase | Dates | Focus | Milestone |
|---|---|---|---|
| P1 — Setup & Exploration | Nov 3 – Nov 9 | Accept rules, read docs, Gemma 4 in Kaggle notebooks | Rules accepted; eval understood |
| P2 — Baseline | Nov 10 – Nov 16 | Fine-tune/post-train baseline bug-fixer; first submission (paper draft Nov 12 if entering) | On the leaderboard |
| P3 — Iterate | Nov 17 – Nov 25 | Tooling/prompt improvements, small-model bonus track | Top-50% position |
| P4 — Finalize | Nov 26 – Dec 2 | Final model selection, write-up, packaging | Final submission in |

## Quickstart

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/01_exploration_baseline.ipynb
```

## Layout

- `notebooks/` — exploration and experiments
- `src/` — reusable training/agent code
- `data/` — competition data (gitignored)
- `submissions/` — submission artifacts
