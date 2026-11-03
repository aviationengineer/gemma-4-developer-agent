# Technical Brief — Gemma 4 Developer Agent Competition

Researched 2026-11-02 from the official Kaggle pages. Rules accepted 2026-11-02 (Kaggle: ehsan4u@gmail.com).

## What the task actually is

SWE-bench-style software engineering: your agent receives a **bug report / feature request**
(`problem_statement` + optional `hints_text`) against a Python repo snapshot at `base_commit`,
and must produce a **unified `git diff` patch** that fixes it. Grading runs the hidden
`test_patch` with pytest: fail-to-pass tests must flip to pass, pass-to-pass must stay green.

- **Training:** 129 public tasks from `fastapi/fastapi`, `Textualize/rich`, `psf/requests`, `encode/httpx`
- **Test:** ~120 tasks, evenly split public/private, curated from **private repos** (so memorization of public repos won't carry you)
- Only the training set is visible; at scoring time it's swapped for the hidden test set

## Data & tooling provided (all offline)

| Asset | What it is |
|---|---|
| `tasks.jsonl` | 129 tasks: `instance_id`, `repo`, `base_commit`, `problem_statement`, `hints_text`, `patch` (reference fix), `test_patch`, `created_at` |
| `snapshots/<id>.tgz` | Full git tree at `base_commit`, future history stripped — safe to run `git log/diff` |
| `graphs/<id>.json` | NetworkX AST call + dependency graphs (directed multigraph; nodes = fully-qualified symbols with full source text; edges = `calls`/imports) |
| `embeddings/<id>.npz` | 256-dim float32 semantic vectors per graph node |
| `wheels/` | 124 pre-compiled offline wheels, mounted read-only at `/wheels/` — `pip install --no-index` works with no internet |
| `docker/` | Sandbox Dockerfiles (Python 3.13, pytest) + `imp`/`telnetlib` shims |
| `sandbox/setup.py` | Per-task container init: resolves deps from `/wheels/`, editable install, clean baseline commit |
| `sample_submission/` | Ready-to-run baseline ADK agent (declarative YAML, multi-agent delegation, multi-LoRA routing) |
| `HARNESS_README.md` | Deep guide: `swegemma`, `adk-submission`, `adk-eval-core`, two-container sandbox lifecycle, tool signatures, context compaction, local CLI eval |

Agent tools available in-sandbox: `get_code_neighbors`, `get_code_subgraph` (graph-backed),
`search_similar_code` (embedding-backed semantic retrieval).

## Submission & scoring

- You submit `submission.zip` — an **ADK agent**, not a CSV. Kaggle runs your agent's inference
  script against the hidden tasks, producing `submission.parquet` (`id`, `prediction` = git diff or `NO_PATCH`).
- Score = fraction of tasks resolved (fail-to-pass flip + pass-to-pass hold), SWE-bench style.
  Public leaderboard on the public test split; prizes on the **private** split.
- **Two final submissions** per team may be selected.

## Prizes & hard dates

| Track | Prize | Deadline |
|---|---|---|
| Main (1st/2nd/3rd) | $37K / $18K / $10K | **Dec 2, 2026** |
| Paper track ($35K) | unpublished research: PEFT/RL tuning, code comprehension, new tasks/benchmarks, graph reasoning | **Nov 12, 2026** — before the main deadline |
| Small-model bonus | $10K, base model must be **Gemma E2B or E4B** (LoRA allowed), eligible for main prizes too | Dec 2, 2026 |

## Suggested strategy

1. **Start from `sample_submission/`** — run the baseline through the local harness CLI first; get a
   scored baseline before touching the model.
2. **Use the graph + embeddings** — explicitly encouraged by Google; semantic retrieval
   (`search_similar_code`) + subgraph navigation is the differentiator over a plain file-search agent.
3. **Small-model track is +EV** — an E2B/E4B + LoRA entry competes for the same $65K pool *and*
   the $10K bonus. Consider training the small model as your main entry.
4. **Paper track runs in parallel** — if entering ($35K), the draft must exist by Nov 12, i.e. during
   the baseline phase. Pick a paper angle early (e.g. graph-reasoning ablations).
5. **Local verification loop** — replicate their pipeline: fail-to-pass on `base_commit`, then
   pass-to-pass with your patch. Never trust a patch that wasn't pytest-verified in the sandbox.

## Rules that bite

- One Kaggle account only; no private code sharing outside your team (public sharing on Kaggle forums is fine).
- Any open-source code in your submission must be OSI-approved licensed.
- Winners are notified by email; prizes net of applicable taxes.
