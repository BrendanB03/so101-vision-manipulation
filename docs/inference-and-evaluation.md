# Inference and Evaluation

## Autonomous rollout

During inference, the follower arm executed the learned ACT policy using the same observation structure established during recording and training. The camera viewed the red cube, blue bin, and arm workspace while the policy generated action chunks for the complete task.

The complete historical rollout command was not recovered. It is intentionally not reconstructed.

Before a rollout:

1. Confirm the follower calibration and serial device.
2. Confirm that the correct D455 stream is active.
3. Verify camera pose and workspace visibility.
4. Remove any teleoperation process that could also command the follower.
5. Load the intended policy checkpoint.
6. Place the cube and bin according to the evaluation protocol.
7. Clear the workspace and begin one autonomous attempt.

## Evaluation progression

| Evaluation | Result | Interpretation |
| --- | ---: | --- |
| Initial generalized test | 11/20 (55.0%) | Strong evidence that the early training distribution was too narrow. |
| Intermediate qualification | 20/21 (95.2%) | Large improvement after targeted dataset refinement; detailed conditions were not recoverable. |
| Expanded-position qualification | 24/25 (96.0%) | Four new top/bottom gap positions succeeded; one trial struck the cube's top. |
| Final randomized acceptance test | 29/30 (96.7%) | Final project result across randomized intermediate positions and orientations. |

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

A low 3D-printed boundary and removable ramp had been designed to randomize cube position and yaw. Available project history does not confirm that the ramp was actually used during the final 30 trials, so it is not listed as part of the verified final protocol.

## Result

\[
\text{Success rate} = \frac{29}{30} \times 100 = 96.7\%
\]

The sole failure occurred on trial 15 at 63 degrees. The recorded note was: **Failed to orient properly.**

## Row-level data status

The project history available during this documentation pass exposed the aggregate result and the single failure, but not all 30 original table rows. Publishing 29 invented success rows would make the repository look complete while weakening its credibility.

The trial-by-trial CSV will be added after the original table is supplied or exported. Until then, [the evaluation summary](../results/evaluation-summary.md) is the authoritative repository record.

## Limitations

- Thirty trials provide useful acceptance evidence but not a broad statistical characterization.
- Trials came from one robot, camera, object, bin, and tabletop setup.
- The test did not independently estimate grasp and placement reliability from the recovered row-level data.
- Lighting and background robustness were not systematically qualified.
- Camera pose sensitivity remained a known limitation.

## Recommended next evaluation

Use at least 100 preregistered trials split across position zones, angle bins, lighting conditions, and multiple sessions. Record grasp success, lift success, bin placement, overall success, failure category, and policy/checkpoint revision separately.

