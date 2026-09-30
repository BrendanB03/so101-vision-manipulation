# Inference and Evaluation

## Autonomous rollout

During inference, the follower arm executed the learned ACT policy using the same observation structure established during recording and training. The camera viewed the red cube, blue bin, and arm workspace while the policy generated action chunks for the complete task.

Before a rollout:

1. Confirm the follower calibration and serial device.
2. Confirm that the correct camera stream is active.
3. Verify camera pose and workspace visibility.
4. Remove any teleoperation process that could also command the follower.
5. Load the intended policy checkpoint.
6. Place the cube and bin according to the evaluation protocol.
7. Clear the workspace and begin one autonomous attempt.

## Evaluation progression

| Evaluation | Result | Interpretation |
| --- | ---: | --- |
| Fixed-position baseline | 10/10 (100%) | Demonstrated repeatability at the taught position and orientation; position and orientation generalization were not yet tested. |
| V2 position-generalization test | 24/30 (80.0%) | Performance varied by workspace region. The right side succeeded in only 1/5 trials, identifying a clear weakness. |
| V5 position qualification | 29/30 (96.7%) | Targeted position recordings improved right-side performance to 5/5. Many successful grasps still favored the cube’s right side. |
| V6 center-orientation test | 14/20 (70.0%) | Testing different orientations at the center exposed right-biased grasp failures, particularly at +75° and −15°. |
| Broader generalized diagnostic | Approximately 11/20 (55.0%) | Broader testing exposed remaining generalization limits before the clean rebuild. |
| Clean 378-episode model qualification | 20/21 (95.2%) | Substantial improvement following the fresh, balanced position-and-orientation recordings. |
| Final 450-episode model qualification | 24/25 (96.0%) | Evaluated the expanded position coverage after adding 72 demonstrations. One failed trial struck the cube’s top. |
| Final randomized acceptance test | **29/30 (96.7%)** | Final reported result across randomized positions and intermediate orientations. The sole failure occurred at 63° and involved improper grasp orientation. |

## Final acceptance-test protocol

Verified elements of the final test:

- 30 trials
- First attempt only
- No retries
- A success required autonomous pickup **and** placement in the blue bin
- Positions included intermediate locations between the 25 training markers
- Cube angles ranged from 4 degrees to 86 degrees
- Previously unseen intermediate orientations were included
- Position, angle, overall success, and notes were recorded

## Result

\[
\text{Success rate} = \frac{29}{30} \times 100 = 96.7\%
\]

The sole failure occurred on trial 15 at 63 degrees. The recorded note was: **Failed to orient properly.**

## Row-level data

The complete source table was preserved as [Final Test Results](../results/final-test-results.csv). It contains all 30 randomized positions, angles, overall outcomes, and original notes.

## Limitations

- Thirty trials provide useful acceptance evidence but not a broad statistical characterization.
- Trials came from one robot, camera, object, bin, and tabletop setup.
- The test did not independently score grasp and placement reliability in separate fields.
- Lighting and background robustness were not systematically qualified.
- Camera pose sensitivity remained a known limitation.
