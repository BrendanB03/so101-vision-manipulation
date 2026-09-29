# Evaluation Summary

## Headline result

| Measure | Value |
| --- | ---: |
| Complete autonomous trials | 30 |
| Successful pickup-and-bin placements | 29 |
| Failures | 1 |
| First-attempt success rate | 96.7% |
| Retries | 0 |
| Cube-angle range | 4–86 degrees |

A success required the SO-101 follower to autonomously pick up the red cube and place it in the blue bin. The test included randomized positions between the 25 training locations and intermediate/unseen orientations.

## Failure record

| Trial | Angle | Result | Recorded note |
| ---: | ---: | --- | --- |
| 15 | 63 degrees | Failure | Failed to orient properly |

## Development progression

| Stage | Successes | Trials | Rate |
| --- | ---: | ---: | ---: |
| Initial generalized evaluation | 11 | 20 | 55.0% |
| Intermediate qualification | 20 | 21 | 95.2% |
| Expanded-position qualification | 24 | 25 | 96.0% |
| Final randomized acceptance test | 29 | 30 | 96.7% |

The 24/25 test's only failure involved the arm striking the top of the cube. All four newly added top/bottom gap positions succeeded.

## Interpretation

The improvement from 55.0% to 96.7% followed targeted dataset changes based on observed failure modes. The strongest supported conclusion is that the final policy generalized well within the qualified tabletop task distribution—not that it solved unrestricted object manipulation.

## Row-level results

The complete source data is available in [final-test-results.csv](final-test-results.csv). The CSV preserves the original randomized-position descriptions, angles, overall success values, and notes.

The original test table did not separately score grasp success and bin-placement success, so those fields are not inferred.
