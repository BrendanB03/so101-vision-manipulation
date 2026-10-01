# Evaluation Summary

The final 450-episode ACT policy completed **29 of 30 first-attempt autonomous pick-and-place trials (96.7%)**. This page records that acceptance result and the earlier tests that guided position coverage, orientation coverage, and demonstration quality.

## Headline result

| Measure | Value |
| --- | ---: |
| Autonomous trials attempted | 30 |
| Successful pickup-and-bin placements | 29 |
| Failed complete tasks | 1 |
| First-attempt success rate | 96.7% |
| Retries | 0 |
| Tested cube-angle range | 4–86 degrees |

A success required the SO-101 follower to autonomously pick up the red cube and place it in the blue bin on the first attempt. The test included randomized positions between the 25 training locations and intermediate orientations beyond the six recorded angle groups.

The final training dataset contained **450 demonstrations: 25 positions × 6 orientations × 3 repetitions**. The recorded training orientations were 0°, 15°, 30°, 45°, 60°, and 75°. See [Dataset Collection](../docs/dataset-collection.md) and [Policy Training](../docs/policy-training.md) for the development and training history.

## Final failure record

| Trial | Angle | Result | Recorded note |
| ---: | ---: | --- | --- |
| 15 | 63 degrees | Failure | Failed to orient properly |

This was the sole failed complete task in the final 30-trial test.

## Development progression

| Stage | Result | Findings |
| --- | ---: | --- |
| Fixed-position baseline | 10/10 (100%) | Demonstrated repeatability at the taught position and orientation; position and orientation generalization were not yet tested. |
| V2 position-generalization test | 24/30 (80.0%) | Performance varied by workspace region. The right side succeeded in only 1/5 trials, identifying a clear weakness. |
| V5 position qualification | 29/30 (96.7%) | Targeted position recordings improved right-side performance to 5/5. Many successful grasps still favored the cube's right side. |
| V6 center-orientation test | 14/20 (70.0%) | Testing different orientations at the center exposed right-biased grasp failures, particularly at +75° and −15°. |
| Broader generalized diagnostic | Approximately 11/20 (55.0%) | Broader testing exposed remaining generalization limits before the clean rebuild. The exact dataset checkpoint was not recovered. |
| Clean 378-episode model qualification | 20/21 (95.2%) | Substantial improvement following fresh, balanced position-and-orientation recordings. |
| Final 450-episode model qualification | 24/25 (96.0%) | Evaluated the expanded position coverage after adding 72 demonstrations. One failed trial struck the cube's top. |
| Final randomized acceptance test | **29/30 (96.7%)** | Final result across randomized positions and intermediate orientations. The sole failure occurred at 63° and was recorded as a failure to orient properly. |

The 24/25 qualification test followed the addition of four positions between top-left/top, top/top-right, bottom-left/bottom, and bottom/bottom-right. All four added positions succeeded in that qualification; its only failure involved the arm striking the top of the cube.

That qualification failure is separate from the final acceptance-test failure at 63°. In the final test, trial 30 slightly contacted the cube's top but still completed pickup and placement successfully.

## Interpretation

The development tests show why fixed-position repeatability, position generalization, and orientation generalization needed separate evaluation. V5 reached 29/30 in its position test, while the later V6 center-orientation test reached 14/20 and exposed a remaining weakness.

The clean rebuild emphasized balanced position-and-orientation coverage and consistent centered grasps. Its qualification results, followed by the final randomized test, support improved behavior within the tested red-cube-to-blue-bin tabletop setup.

These evaluations used different starting conditions, datasets, and sample sizes. Their percentages describe separate development milestones rather than a directly comparable or steadily increasing performance curve. 

Several successful final trials still recorded left- or right-biased grasps, and trial 30 recorded contact with the cube's top. The success rate measures completion of the full task; it does not establish perfectly centered grasps or contact-free motion.

The final 30-trial result supports generalization across the tested positions and orientations in one controlled setup. It does not establish reliability across different objects, camera placements, lighting, backgrounds, or destinations.

## Row-level results and provenance

The complete final acceptance-test data is available in [final-test-results.csv](final-test-results.csv). The CSV preserves all 30 original randomized-position descriptions, angles, overall success values, and notes. It supports the 29 successes, one failure at trial 15, and tested angle range of 4–86 degrees.

Earlier development results are summarized from the project records. Their complete trial tables and exact checkpoint identities are not contained in this repository. The final CSV contains only the final acceptance test; earlier qualifications are separate test sets.


