# Oroboros

**Your job, minus what AI absorbs, plus what you can grow into.**

> Oroboros shows you which parts of your own job AI may eat, under slow and fast scenarios,
> and which real career the leftover can grow into.

A job is a bundle of tasks. AI does not delete jobs overnight: it eats some tasks inside them,
and what is left becomes a different job. Oroboros walks one person through that process:

| Step | What happens | Must-have covered |
|---|---|---|
| **1. Your job** | Pick an O*NET occupation (923 with task lists), tick the tasks you really do, set hours. Optional reflection cards and CV-text paste. | Skills intake |
| **2. Radar** | Momentum for 10 AI capability areas from arXiv, HN hiring threads, GitHub and job boards, refreshed nightly, every number dated. | AI-trend signal layer |
| **3. Consume** | Each task coloured consumed, shrinking, safe or uncertain. The **Week Ring** is eaten live as you drag *Slow drift ↔ Fast Shockwave*. | Uncertainty |
| **4. Regrow** | Every real occupation ranked by `Fit × Resilience`, with the printed score, the gap, the tasks that drove the match, a two-role composite and no-regret skills. | Future-role matching |
| **5. Plan & SDGs** | Ordered steps with verified free courses, hours, weeks at your pace, a proof task per step, SDG tags for each role and skill, and a Purpose slider. | Reskilling pathway, SDG mapping |

Built for the **Aiversecity Future-Ready Workforce** hackathon (Domain 1). Synthetic personas only.

## Team and submission

| Item | Details |
|---|---|
| Team | **fourgee** — 4 members |
| Team leader | KHAIRUL ZARIFF ADEEB BIN KHAIRUL ZIAD |
| University | Universiti Teknologi MARA |
| Domain | Domain 1 — Aiversecity Future-Ready Workforce |
| Primary SDG target | SDG 4.4 — relevant skills for employment, decent jobs and entrepreneurship |
| Submission repository | https://github.com/romaaamaker/oroboros |
| Team prototype URL | https://romaaamaker.github.io/oroboros/ |
| Original live prototype | https://abdurahim-azami.github.io/oroboros/ |
| Pitch deck PDF | [Download PDF](submission/SDC2026_fourgee_Oroboros_Pitch.pdf) |
| YouTube pitch and prototype demo | Pending — add the final 5–7 minute video link before submission |

This repository is the team's submission fork of
[the original Oroboros implementation](https://github.com/abdurahim-azami/oroboros).
Preserve the existing commit history, contributor attribution and third-party
license notices. Proposed impact targets are not measured outcomes.

## Actual tool and sponsor usage

- **Claude Code:** used for development; the exact version was not recorded.
- **Higgsfield AI:** used for web-app visual design.
- **Replit:** used to prepare submission documentation, the pitch deck and the
  pitch/demo video script. Replit does not host the existing public prototype.
- **RunPod:** not used; the project has no GPU workload. The team reports that
  it spoke with the administration and confirmed that RunPod is not required
  for this project. No GPU-use claim is made.

The optional model/server code described below is not an active service in
the submitted static prototype.

## Deploy this fork to GitHub Pages

The included `.github/workflows/pages.yml` deploys this repository's own
`site/` directory; it does not depend on the original repository's deployment.

1. In this repository's **Actions** tab, enable workflows if GitHub prompts.
2. In **Settings → Pages → Build and deployment**, select **GitHub Actions**.
3. In **Actions → Deploy site to GitHub Pages**, choose **Run workflow** on `main`.
4. Wait for the tests and deployment to succeed, then open the URL reported by
   the deployment. For this fork, the expected default URL is
   `https://romaaamaker.github.io/oroboros/`.
5. The site is deployed; recent Pages deployment runs succeeded.

Future pushes to `main` trigger deployment. The workflow also listens for a
successful **Nightly signals** run. Scheduled workflows may need to be enabled
separately in a fork; verify their runs before claiming automatic refreshes.
Repository settings, secrets and collaborator access are not inherited.

GitHub Pages hosts only the static app, which works without an LLM. It does
not run the optional Python/FastAPI server.

## Submission files

Place the final reviewed PDFs and genuine proof of use in `submission/`:

```text
submission/
  SDC2026_fourgee_Preliminary.pdf
  SDC2026_fourgee_TechDeclaration.pdf
  SDC2026_fourgee_Oroboros_Pitch.pdf
  SDC2026_fourgee_Replit_POU.pdf
```

The uploaded pitch deck can be opened at
[submission/SDC2026_fourgee_Oroboros_Pitch.pdf](submission/SDC2026_fourgee_Oroboros_Pitch.pdf).
Complete required signatures and replace the pending video entry before
submission. Invite the committee's designated GitHub accounts as collaborators.
Keep real development history; do not backdate or manufacture commits.

---

## Run it

```bash
# 1. The site (no build step)
python -m http.server 8765 --directory site
# open http://localhost:8765

# 2. Tests (no install needed)
node --test tests/engine.test.js
```

Refresh the live signals and rebuild everything the site reads:

```bash
python -m venv .venv && .venv/Scripts/activate        # Windows; use bin/activate on Mac/Linux
pip install -r pipeline/requirements.txt
python pipeline/run_all.py --full     # first time: downloads O*NET 31.0 and the exposure datasets
python pipeline/run_all.py            # nightly: one command, fresh dated snapshot
```

Optional LLM layer (the app works fully without it):

```bash
pip install -r server/requirements.txt
cp server/.env.example server/.env    # add your Runpod endpoint
uvicorn server.app:app --port 8000
# open http://localhost:8765/?api=http://localhost:8000
```

## How it works (three formulas, printed in the UI)

```
A. Momentum      m_c = clamp( last 90 days ÷ past-year average per 90 days, 0.8, 1.2 )
B. Absorption    A_t = min(1, E_t × m_c × s)        s: Slow 0.6 · Gradual 1.0 · Shockwave 1.6
                 consumed ≥ 0.6 · shrinking 0.35–0.6 · safe < 0.35 · uncertain if the band crosses a line
C. Ranking       Rank = Fit × Resilience            (× (1 + p·S) only when the Purpose slider is used)
```

Full method, every assumption and every threshold: [docs/methodology.md](docs/methodology.md).

## Data sources

| Layer | Source | Update |
|---|---|---|
| Occupations and tasks | O*NET 31.0 (1,016 occupations, 18,838 tasks, DWA hierarchy), CC BY 4.0 | Version stamp |
| Task exposure | GPTs are GPTs labels (MIT), Anthropic Economic Index (data: CC-BY; code: MIT), Felten AIOE (context) | Release-based, re-ingested on change |
| Live momentum | arXiv API, HN Algolia API, GitHub REST API, Remotive and Arbeitnow | **Nightly GitHub Action**, dated snapshots in `data/snapshots/` |
| SDG targets | UN Stats SDG API (169 targets, Malaysia area 458 indicators) | Dated pull |
| Courses | Microsoft Learn Catalog API (23 free learning paths) | Nightly link check |

Details, versions, licences and fetch dates: [docs/data-sources.md](docs/data-sources.md).

## Language model disclosure

- **The core runs without an LLM.** Every number, role, task, course and SDG tag on screen is computed by code from the data files.
- Optional model integration: **Qwen2.5-7B-Instruct** (open weights, Apache-2.0), designed for an OpenAI-compatible vLLM endpoint. It is **not connected in the submitted static prototype**, and RunPod was not used.
- If separately configured, this integration is intended for two jobs only: mapping pasted CV text to task IDs from the chosen occupation's list, and rephrasing sentences the engine already computed.
- Guardrails in `server/app.py`: IDs not in the database are dropped; an explanation containing a number or link not present in the input is rejected and the computed text is shown instead.

## Known limits (said before anyone asks)

1. **O*NET is US-based.** Malaysian titles map through ISCO-08, the base of MASCO 2020. The match is close, not exact.
2. **Exposure is not job loss.** The datasets say which tasks an AI system could speed up.
3. **Exposure datasets update by release, not daily.** Only the momentum layer is nightly.
4. **Robots are under-counted.** The exposure data measure language-model AI; physical work can look safer than it is.
5. **Momentum, scenario speeds and thresholds are our stated assumptions**, printed on every screen.
6. **The jobs feed is young.** It joins momentum after 90 days of nightly history.
7. **Replay snapshots** are rebuilt from real historical counts (arXiv dates, HN threads, GitHub weekly stats), and are labelled "replay".

## Repository map

```text
site/                 the website (index.html + assets/ + data/), deployable as-is
  assets/js/engine.js the scoring engine: every formula, pure functions, tested
  assets/js/app.js    the five screens
  assets/js/site.js   scroll-scrubbed hero, captions, motes, reveals
  data/               generated JSON (occupations, roles, momentum, replay, courses, sdg, personas)
pipeline/             Python: fetch, DuckDB load, ingest scripts, momentum, courses, SDG, export
  ingest/             arxiv.py · hn.py · github.py · jobs.py
  config/             capabilities, reviewed maps, personas, curated courses (all hand-editable)
data/snapshots/       dated nightly snapshots (source, fetch time, row count)
data/*.csv            hand-off files: task_capability_map.csv, sdg_map_reviewed.csv
server/               optional FastAPI LLM layer with validators
tests/                engine acceptance tests (node) + server guardrail tests (pytest)
docs/                 methodology, data sources, demo script, proposal, team guide, design package
.github/workflows/    nightly ingest, link check, GitHub Pages deploy
```

## Rules we follow

Public, open or synthetic data only. No real CVs. Every recommendation cites a source. No
invented jobs, courses or statistics: composite roles are always two real, named O*NET
occupations, and courses come only from the catalogue with a URL and a check date. Oroboros
assists a person's own choice; it must not be used for hiring or firing.

Attribution and licences: [LICENSE_NOTICES.md](LICENSE_NOTICES.md).

## Licensing

Team-owned original material is licensed under [Apache-2.0](LICENSE), excluding third-party assets and datasets. See [LICENSE_NOTICES.md](LICENSE_NOTICES.md). **Outstanding:** AIOE has no explicit upstream redistribution license; citation alone is not clearance. Anthropic specifies CC-BY for data and MIT for code; its metadata differs, so verify release-specific terms before asserting full compliance.
