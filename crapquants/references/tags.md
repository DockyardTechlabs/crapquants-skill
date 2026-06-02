# CRAPQuants Diagnostic Tags Reference

## Feathers Tags (Testability Risk)
| Tag | Triggers When | Action |
|-----|--------------|--------|
| MONSTER_SNARLED | CC>10, nesting≥4 | Replace Method with Method Object |
| MONSTER_BULLETED | CC>10, lines≥50, low nesting | Extract Method per section |
| EDIT_AND_PRAY | 0% coverage, CC>5 | Write Characterization Tests first |
| CHARACTERIZATION_NEEDED | CRAP>30, 0% coverage | Use Feathers' assert-wrong-then-learn algorithm |
| LEGACY_DILEMMA | CRAP>30, params≥4 | Extract Interface/Protocol, then test |
| REFACTOR_MANDATORY | CC≥31 | Must reduce CC — testing alone cannot help |
| SENSING_PROBLEM | params≥3, CC>5, cov<50% | Separate Query from Modifier |

## Ousterhout Tags (Design Quality)
| Tag | Triggers When | Action |
|-----|--------------|--------|
| SHALLOW_MODULE | depth_ratio<3, ≤5 lines | Inline Method or deepen functionality |
| OVEREXPOSED_API | params>4 | Introduce Parameter Object |
| PASS_THROUGH | ≤3 lines, CC=1, single call | Inline Method |
| VAGUE_NAMING | Name contains data/result/temp/handler/etc | Rename Method |
| HARD_TO_NAME | Name >40 chars or contains _and_/_or_ | Extract Method — does too much |
| NONOBVIOUS | Multiple signals (CC>5, CogC>10, params>3, nesting≥3) | Introduce Explaining Variable + Extract Method |
| COMPLEXITY_UPWARD | Conditions >40% of ABC | Define Errors Out of Existence |
| DEEP_MODULE | depth_ratio≥10, CRAP<30 | Positive signal — good design |

## Hunt & Thomas Tags (Culture)
| Tag | Triggers When | Action |
|-----|--------------|--------|
| COINCIDENCE_CODE | CC≥8, 0% cov, nesting≥2 | Write tests + Extract Method + Introduce Assertion |
| NO_SAFETY_NET | 0% coverage, CC>5 | Write tests before next change |
| NO_CONTRACTS | CC>10, few conditions | Add pre/postcondition assertions |
| BROKEN_WINDOW | CRAP>30, cov<20% | Fix first — any improvement breaks rot cycle |
| REFACTOR_READY | CRAP>30, cov≥70% | Safe to refactor — coverage is safety net |

## Fowler Tags (Smells → Refactorings)
| Tag | Triggers When | Action |
|-----|--------------|--------|
| LONG_METHOD | lines>30 or CC>10 | Extract Method |
| LONG_PARAM_LIST | params>4 | Introduce Parameter Object |
| CONDITIONAL_CHAIN | High CC, shallow nesting | Replace Conditional with Polymorphism |
| COMMENTS_AS_DEODORANT | CogC>15, CC>8 | Extract Method (name from intent) |
| FEATURE_ENVY | calls >75% of ABC | Move Method to the class it calls most |
| LAZY_CLASS | ≤3 lines, CC=1 | Inline Method |

## Tornhill Tags (Behavioral)
| Tag | Triggers When | Action |
|-----|--------------|--------|
| HOTSPOT_DORMANT | CRAP>60, cov<10% | Monitor — time bomb but not urgent |
| KNOWLEDGE_SILO | CogC>20, CRAP>30 | Pair programming + documentation |

## Ford Tags (Fitness Functions)
| Tag | Triggers When | Action |
|-----|--------------|--------|
| FITNESS_CRAP_EXCEEDED | CRAP > threshold | Reduce CC or increase coverage |
| FITNESS_COGC_EXCEEDED | CogC > 15 | Decompose Conditional + Extract Method |
| FITNESS_ABC_EXCEEDED | ABC scalar > 30 | Extract Method to reduce A/B/C |
