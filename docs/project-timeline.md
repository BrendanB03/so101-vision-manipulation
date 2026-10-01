# Project Timeline

This timeline traces the SO-101 red-cube-to-blue-bin project from hardware preparation through ACT training, physical testing, the printed ramp, and portfolio documentation. The completed policy used a **450-episode final dataset** and achieved **29/30 first-attempt complete tasks (96.7%)** in the final randomized acceptance test.

## July 23 to 30 2026 — Planning and printed hardware

- Defined the goal of moving a red cube into a blue bin using an SO-101 follower, a leader for demonstrations, and a workspace camera.
- Purchased required build components and prepared a dedicated Ubuntu workstation. The recorded workstation included an RTX 2080 Super and 16 GB RAM.
- Organized the local project and hardware reference workspaces.
- Completed the printed leader and follower part sets in PLA by July 30.
- Planned the progression through motor configuration, assembly, calibration, teleoperation, camera integration, recording, training, and autonomous evaluation.

## August 1 to 3 — Motor configuration and arm assembly

- Configured six motors for each arm, covering base, shoulder, elbow, wrist flex, wrist roll, and gripper.
- Assembled the follower joints, then completed controller mounting, wiring, and the leader/follower build.
- Established serial communication and initial leader-follower motion.
- Continued calibration and motion verification before demonstration collection.

## August 4 to 8 — Camera and recording pipeline

- Integrated the Intel RealSense D455 and verified an RGB workspace view that included the cube, gripper approach, and blue bin.
- Early camera tests used a 640×480 color stream at 30 FPS, discovered at device index 4. Device indices were specific to that setup.
- Used LeRobot and LeLab for teleoperation, synchronized camera/state/action recording, dataset management, training, and rollout.
- Recorded the five-episode pipeline test
- Inspected the recorded videos before moving into a fixed-position training dataset.

## August 8 to 9 — Fixed-position baseline and first generalization test

- Trained and deployed an initial ACT policy for one consistent cube position and orientation.
- Achieved **10/10 repeatability** in the fixed setup. This established the working recording-to-rollout pipeline.
- Recorded V2 with varied cube positions while retaining one orientation.
- V2's regional position test achieved **24/30 (80.0%)**. The right side succeeded in **1/5** trials, exposing uneven spatial coverage.

## August 10 to 11 — Targeted position coverage and V5 merge

- Collected additional demonstrations around weak right-side locations and intermediate positions.
- The V3 source repository was 100 episodes.
- Merged V2 and V3 into V5 using LeRobot dataset tools. 
- Retained source datasets while validating the merged result.

## August 13 — Randomized reset concept

- Proposed a low workspace boundary and removable ramp to let the cube enter the pickup area with varying position and yaw.
- Planned hands-off randomized testing within the reachable workspace, followed by a quantitative result and demonstration video.
- The ramp was a proposed test fixture at this point; its later printed completion is recorded separately below.

## August 15 — Position qualification and first orientation dataset

- V5 achieved **29/30 (96.7%)** in its position-focused qualification: top, left, right, bottom, and center were each 5/5; random positions were 4/5.
- Right-side regional performance improved from V2's 1/5 to 5/5, but many successful grasps were biased toward the cube's right side.
- Added **60 orientation demonstrations** 
- Formed the **240-episode V6 dataset** from V2 80 + V3 100 + orientation 60.
- Encountered missing episode metadata in an earlier merged repository, including a referenced `file-001.parquet`. Rebuilding from intact source datasets became necessary.

## August 16 — Orientation diagnostics and V7

- The V6 center-orientation test achieved **14/20 (70.0%)**, with right-biased misses particularly at +75° and −15°.
- A separate fixed-center retest reported **10/10 pickups**, although grasp bias remained.
- A separate randomized position-and-orientation test reported **14/30 pickups (46.7%)**, including lateral misses, top contacts, and orientation failures. 
- Recorded a further **60 targeted orientation demonstrations**, emphasizing difficult angles and weak workspace regions.
- V7 merged all previous datasets totaling **300 episodes**

These tests showed that repeatable center behavior and stronger position coverage did not establish reliable combined position-and-orientation performance.

## August 16 — Further grasp correction and the 360-episode merge

- Added **60 corrective demonstrations** aimed at persistent rightward grasp bias, with an emphasis on centered grasps.
- V8 was made up of the five source datasets combined into **360 episodes** 

The 240, 300, and 360-episode versions reused earlier recordings. Their counts are not additive.

## During August iteration — Camera geometry and broader testing

- The D455 was bumped and repositioned by eye during development. 
- The pose change reduced comparability between older and newer demonstrations and likely contributed to directional grasp bias.
- A broader diagnostic was reported at approximately **11/20 (55.0%)** before the clean rebuild. Its exact date and checkpoint were not recovered.
- These issues reinforced the need for stable camera geometry, deliberate position/orientation coverage, and consistent demonstration quality.

## August 22 — Fresh balanced recording series

- Started a clean dataset series that excluded the earlier accumulated demonstrations.
- Recorded six orientation-specific datasets at **0°, 15°, 30°, 45°, 60°, and 75°**.
- Each contained **63 episodes: 21 positions × 3 repetitions**.
- Merged them into **378 episodes**
- Focused on balanced spatial coverage and clean centered grasps instead of continuing to append corrections to the old distribution.

The resulting clean model achieved a reported **20/21 qualification result (95.2%)** during the August 22–23 qualification period.

## August 23 — Missing positions and final 450-episode training

- Recorded **72 additional demonstrations** at four gaps: between top-left/top, top/top-right, bottom-left/bottom, and bottom/bottom-right.
- Covered all six orientations with three repetitions at each added position.
- Confirmed **450 episodes** in `TripleB3/rc-2-final-450_20260823`: 25 positions × 6 orientations × 3 repetitions.
- Reported final ACT training from scratch with **100,000 steps, batch size 8, one NVIDIA L40S, image transforms disabled, and 10,000-step checkpoints**.

## August 23 — Qualification and final acceptance

- The expanded 450-episode model achieved **24/25 (96.0%)** in qualification. All four newly added gap positions succeeded; the failed trial struck the cube's top.
- Completed **30 randomized first-attempt acceptance trials with no retries**, testing intermediate positions and cube angles from **4° to 86°**.
- Achieved **29 complete autonomous pickup-and-bin placements out of 30 (96.7%)**.
- The sole failure was **trial 15 at 63°**, recorded as “Failed to orient properly.”
- Trial 30 contacted the cube's top but still completed successfully; it was distinct from the earlier qualification failure.

See the [Evaluation Summary](../results/evaluation-summary.md) and [complete final trial record](../results/final-test-results.csv). Development tests used different conditions and denominators; the percentages are separate milestones.

## August 31 to September 7 — GitHub documentation

- Established a dedicated GitHub documentation workflow for `BrendanB03/so101-vision-manipulation`.
- The early repository inspection found a README and `.gitignore`; the project needed structured setup, dataset, training, evaluation, and troubleshooting records.
- Documentation PR #1 was created and merged on **September 7**, adding the project narrative and supporting documents.
- PR #2 was opened on **September 7** to preserve the complete 30-trial acceptance record.

## September 9 to 15 — Ramp and final demonstration work

- Began the dedicated final-ramp-video discussion and planned a short demonstration of the finished system.
- By the mid-September project review, the ramp had been designed, 3D-printed, and reported working in the finished setup.
- Prepared to show the demonstration workflow, autonomous full cycle, randomized cube starts, and measured result.
