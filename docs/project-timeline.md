# Project Timeline

This timeline consolidates the available Robot Arm project history. Dates identify documented milestones rather than every work session.

## July 2026 — Foundation

- Defined the SO-101 vision-manipulation project.
- Prepared the Ubuntu workstation and initial GitHub repository.
- Assembled the leader and follower arms.
- Established serial communication and calibration.
- Selected LeRobot and ACT for the first learned pick-and-place policy.

## Early August 2026 — End-to-end pipeline

- Connected the Intel RealSense D455.
- Implemented leader-follower teleoperation.
- Recorded the red-cube-to-blue-bin behavior.
- Trained and deployed an initial ACT policy.
- Demonstrated strong controlled behavior, including a recorded 10/10 bench check.
- Identified poor workspace generalization as the next major problem.

## August 10–13 — Position coverage

- Expanded demonstrations across varied positions.
- Organized coverage around 25 training locations.
- Evaluated positions outside the most common training distribution.
- Observed an initial generalized result of 11/20 (55.0%).
- Added targeted demonstrations for sparse and right-side areas.
- Developed the concept of a low boundary and removable ramp for random placement and yaw.

## August 11–16 — Dataset iteration and orientation coverage

- Progressed through dataset snapshots referenced at 125 and 175 episodes.
- Identified orientation generalization as a separate weakness.
- Added orientation-focused demonstrations.
- Combined compatible datasets into a 240-episode version 6 dataset.
- Encountered a missing episode-metadata parquet file in an earlier merged repository.
- Rebuilt the dataset as a validated 300-episode version 7 snapshot.

## August 16–17 — Final recorded ACT training

- Trained ACT using the rebuilt 300-episode dataset.
- Resumed from a 70,000-step checkpoint.
- Completed the 100,000-step target.
- Recorded batch size 8, 97,030 frames, and approximately 51.6 million parameters.
- Continued real-world testing rather than selecting a policy from loss alone.

## August 23 — Qualification and completion

- Recorded an intermediate 20/21 qualification result.
- Expanded the position test to 24/25; all four newly added gap positions succeeded.
- Ran a 30-trial randomized first-attempt acceptance test with no retries.
- Tested intermediate positions between training markers and cube angles from 4 degrees to 86 degrees.
- Achieved 29/30 successful complete tasks (96.7%).
- Recorded the only failure on trial 15 at 63 degrees: the policy failed to orient properly.
- Declared the beginner SO-101 project complete and moved to portfolio documentation.

## September 2026 — Repository documentation

- Connected the private GitHub repository to Codex.
- Confirmed that the original repository contained only a placeholder README and `.gitignore`.
- Chose a documentation-first repository rather than fabricating a custom codebase.
- Preserved dataset and policy privacy while preparing a reviewable portfolio narrative.

