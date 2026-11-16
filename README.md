# Seeing Weight

**Can a small physics-trained model beat foundation models at weighing a ball from video?**

In flight, a video shows only drag divided by mass. Gravity gives metric scale, and a sphere's
drag coefficient is tightly bounded, so mass can be solved from the flight arc alone
(`m = ρ C_d A / 2k`). We compare a small model trained only on simulated throws against large
VLMs and appearance-only estimators, on real balls with bench-measured mass, restitution and
friction, including lookalike pairs that look identical but differ in mass.

## Hypotheses

- **H1** The small physics model beats large VLMs and a fine-tuned VLM on mass, restitution and friction error.
- **H2** On lookalike pairs, VLMs and appearance-only models are fooled (mass ratio ≈ 1); the physics model separates the pair.
- **H3** Mass error from motion falls as drag grows relative to gravity (beach ball easy, medicine ball not identifiable).
- **H4** A hybrid (appearance prior + motion posterior) beats either alone.
- **H5** *(launcher add-on)* After one filmed throw, the physics model aims the second throw better than appearance or a landing-offset correction.

## Layout

| Path | What | Owner (role) |
|---|---|---|
| `src/seeing_weight/tracking/` | Ball detector, Kalman filter, free-flight segments, gravity scale, drag fit | Vision |
| `src/seeing_weight/sim/` | Differentiable ball simulator (drag, Magnus, bounces), domain-randomized generator | Physics model |
| `src/seeing_weight/model/` | Posterior network, CMA-ES refinement, hybrid | Physics model |
| `src/seeing_weight/baselines/` | Zero-shot VLMs, fine-tuned VLM, appearance-only mass | Baselines |
| `src/seeing_weight/evaluation/` | Metrics, bootstrap CIs, permutation tests, figures | Shared |
| `scripts/` | Command-line entry points (run tracker, generate sim data, run baselines) | — |
| `notebooks/` | Exploration only; anything kept moves into `src/` | — |
| `data/` | Local only, see [data/README.md](data/README.md) | Balls & ground truth |
| `paper/` | LaTeX source for the preprint | Shared |

## Setup

```bash
uv sync            # creates .venv and installs dependencies
uv run pytest      # sanity check
cp .env.example .env   # add API keys for the VLM baselines (Week 5)
```

## Working rules

- Sessions: Mon, Wed, Fri 7–9pm and Sun 2–5pm. Each session has one deadline.
- A deadline counts as **done only when pushed here or logged in the shared sheet**. Put the commit link in the sheet's Sessions tab.
- Missed deadline → Saturday make-up (2–4pm). Still open at Sunday's session → first thing Monday.
- Sunday ends with a 10-minute check against the weekly goal. Two missed weekly goals in a row → cut scope (launcher add-on, then fine-tuned VLM, then hybrid). The 10 Jan 2027 arXiv date never moves.
- Never commit videos, API keys or `.env`.

## Timeline

| Week | Dates | End goal |
|---|---|---|
| 1 | Nov 16–22 | All balls and lookalike pairs ready; ground truth measured; pilot clips filmed |
| 2 | Nov 23–29 | Tracker and gravity scale work; simple drag fit — **go/no-go 1** |
| 3 | Nov 30–Dec 6 | Simulator with drag, spin and bounces; simulated dataset generated |
| 4 | Dec 7–13 | Physics network trained; mass-vs-drag curve on sim data |
| 5 | Dec 14–20 | VLM and appearance-only baselines run on pilot clips |
| 6 | Dec 21–27 | Full capture (300+ clips); test set frozen — **go/no-go 2** |
| 7 | Dec 28–Jan 3 | All models evaluated; figures done |
| 8 | Jan 4–10 | Paper posted to arXiv |
| 9–12 | Jan 11–Feb 7 | Launcher add-on → arXiv v2, IROS 2027 |

Session-by-session deadlines, roles, equipment and all measurements are in the shared tracker sheet.
