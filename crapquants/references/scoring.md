# CRAPQuants Scoring Reference

## CRAP Formula
```
CRAP(m) = CC² × (1 − coverage/100)³ + CC
```

## Threshold Table (CC → Minimum Coverage Needed)
| CC | Min Coverage to Stay Under CRAP 30 |
|----|-----------------------------------|
| 1–5 | 0% |
| 6–10 | ~42% |
| 11–15 | ~57% |
| 16–20 | ~71% |
| 21–25 | ~80% |
| 26–30 | ~100% |
| 31+ | Impossible — must reduce CC first |

## Composite Scores
| Score | Source | Formula |
|-------|--------|---------|
| FRS (Feathers Risk) | Testability | CRAP × monster_mult × dep_depth × responsibility |
| ORS (Ousterhout Risk) | Design quality | CRAP × depth_factor × obscurity × red_flags |
| TBS (Tornhill Behavioral) | Activity risk | CRAP × activity × trend × knowledge × coupling |
| PHS (Pragmatic Health) | Codebase-level | 100 minus penalties for CRAPpy/broken windows/coincidence |
| CQ_score | Final | max(FRS, ORS, TBS) — worst assessment wins |

## CRAPload
Minimum work to fix: uncovered_paths + refactoring_count.
CC < 31: add tests. CC ≥ 31: must reduce complexity first.
