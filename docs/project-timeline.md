# Project Timeline

This timeline traces the SO-101 red-cube-to-blue-bin project from hardware preparation through ACT training, physical testing, the printed ramp, and portfolio documentation. The completed policy used a **450-episode final dataset** and achieved **29/30 first-attempt complete tasks (96.7%)** in the final randomized acceptance test.

Dates identify recorded milestones or approximate work periods. Early version labels and counts are reconciled against the current [Dataset Collection](dataset-collection.md) ledger; uncertain dates and remaining discrepancies are identified below.

## July 23 to 30 2026 — Planning and printed hardware

- Defined the goal of moving a red cube into a blue bin using an SO-101 follower, a leader for demonstrations, and a workspace camera.
- Reported purchasing the required build components and preparing a dedicated Ubuntu workstation. The recorded workstation included an RTX 2080 Super and 16 GB RAM.
- Organized the local project and hardware reference workspaces.
- Completed the printed leader and follower part sets in PLA by July 30.
- Planned the progression through motor configuration, assembly, calibration, teleoperation, camera integration, recording, training, and autonomous evaluation. These were planning milestones, not evidence that every software stage was already complete.

## August 1 to 3 — Motor configuration and arm assembly

- Configured six motors for each arm, covering base, shoulder, elbow, wrist flex, wrist roll, and gripper.
- Assembled the follower joints, then completed controller mounting, wiring, and the leader/follower build.
- Established serial communication and initial leader-follower motion. Historical device assignments were follower `/dev/ttyACM0` and leader `/dev/ttyACM1`.
- Continued calibration and motion verification before demonstration collection. The exact completion date of calibration was not preserved; both arms were calibrated before recording.

## August 4 to 8 — Camera and recording pipeline

- Integrated the Intel RealSense D455 and verified an RGB workspace view that included the cube, gripper approach, and blue bin.
- Early camera tests used a 640×480 color stream at 30 FPS, discovered at device index 4. Device indices were specific to that setup.
- Used LeRobot and LeLab for teleoperation, synchronized camera/state/action recording, dataset management, training, and rollout.
- Recorded the five-episode pipeline test on August 8: `TripleB3/red-cube-blue-bin-test_20260808_125244`.
- Inspected the recorded videos before moving into a fixed-position training dataset.

## August 8 to 9 — Fixed-position baseline and first generalization test

- Trained and deployed an initial ACT policy for one consistent cube position and orientation.
- Achieved **10/10 repeatability** in the fixed setup. This established the working recording-to-rollout pipeline.
- Recorded V2 with varied cube positions while retaining one orientation. The first reported count was 75; the source repository later showed **80 episodes**.
- V2's regional position test achieved **24/30 (80.0%)**. The right side succeeded in **1/5** trials, exposing uneven spatial coverage.

The current dataset ledger lists **25 fixed-position baseline episodes**; older chat notes referred to 50. This count remains a historical discrepancy, and the timeline follows the current ledger.

## August 10 to 11 — Targeted position coverage and V5 merge

- Collected additional demonstrations around weak right-side locations and intermediate positions.
- The V3 source repository later showed **100 episodes**.
- Merged V2 and V3 into V5 using LeRobot dataset tools. The current ledger lists **180 episodes**, matching the later 80 + 100 source counts.
- Corrected a merge-command issue: the installed tool expected `--new_repo_id` for the output dataset.
- Retained source datasets while validating the merged result.

Earlier discussions expected 105 episodes from then-reported 75 + 30 counts and also mentioned 125- and 175-episode stages. Those earlier figures were not reliably mapped to distinct final snapshots; they are not additional recording totals.

## August 13 — Randomized reset concept

- Proposed a low workspace boundary and removable ramp to let the cube enter the pickup area with varying position and yaw.
- Planned hands-off randomized testing within the reachable workspace, followed by a quantitative result and demonstration video.
- The ramp was a proposed test fixture at this point; its later printed completion is recorded separately below.

## August 15 — Position qualification and first orientation dataset

- V5 achieved **29/30 (96.7%)** in its position-focused qualification: top, left, right, bottom, and center were each 5/5; random positions were 4/5.
- Right-side regional performance improved from V2's 1/5 to 5/5, but many successful grasps remained biased toward the cube's right side.
- Added **60 orientation demonstrations** in `TripleB3/red-cube-orientation-v6_20260815_190736`.
- Formed the **240-episode V6 dataset** from V2 80 + V3 100 + orientation 60.
- Encountered missing episode metadata in an earlier merged repository, including a referenced `file-001.parquet`. Rebuilding from intact source datasets became necessary.

## August 16 — Orientation diagnostics and V7

- The V6 center-orientation test achieved **14/20 (70.0%)**, with right-biased misses particularly at +75° and −15°.
- A separate fixed-center retest reported **10/10 pickups**, although grasp bias remained.
- A separate randomized position-and-orientation test reported **14/30 pickups (46.7%)**, including lateral misses, top contacts, and orientation failures. This early result measured reported pickups; it is not treated as the final complete pickup-and-bin-placement metric.
- Recorded a further **60 targeted orientation demonstrations**, emphasizing difficult angles and weak workspace regions.
- Rebuilt V7 directly from compatible sources, producing **300 episodes** in `TripleB3/red-cube-v7-rebuilt_20260816`.

These tests showed that repeatable center behavior and stronger position coverage did not establish reliable combined position-and-orientation performance.

## August 16 onward — V7 training interruption and recovery

- The recorded V7 ACT run used **300 episodes and 97,030 frames**, with effective batch size 8 and a 100,000-step target.
- A training interruption occurred around 73,000 steps; a valid 70,000-step checkpoint was available.
- The saved August 16 log shows resumption from `070000`. Its excerpt records resumed startup rather than proving completion of the target.
- The detailed ResNet-18/transformer configuration and approximately 51.6 million parameters belong to this earlier V7 experiment.

See [Policy Training](policy-training.md) for the recorded configuration. V7 was an intermediate experiment; the later final 450-episode run started from scratch.

## August 16 — Further grasp correction and the 360-episode merge

- Added **60 corrective demonstrations** aimed at persistent rightward grasp bias, with an emphasis on centered grasps.
- Combined the five source datasets into **360 episodes** in `TripleB3/red-cube-v9-combined_20260816`.
- The current ledger calls the added recording stage **V8**. Its historical source repository was named `TripleB3/red-cube-v9_20260816_151902`; the recording and combined dataset are separate artifacts.

The 240-, 300-, and 360-episode versions reused earlier recordings. Their counts are not additive.

## During August iteration — Camera geometry and broader testing

- The D455 was bumped and repositioned by eye during development. The exact date was not recovered.
- The pose change reduced comparability between older and newer demonstrations and likely contributed to directional grasp bias.
- A broader diagnostic was reported at approximately **11/20 (55.0%)** before the clean rebuild. Its exact date and checkpoint were not recovered.
- These issues reinforced the need for stable camera geometry, deliberate position/orientation coverage, and consistent demonstration quality.

This diagnostic and the V6 14/30 test were separate evaluations and are not combined into one result.

## August 22 — Fresh balanced recording series

- Started a clean dataset series that excluded the earlier accumulated demonstrations.
- Recorded six orientation-specific datasets at **0°, 15°, 30°, 45°, 60°, and 75°**.
- Each contained **63 episodes: 21 positions × 3 repetitions**.
- Merged them into **378 episodes** in `TripleB3/rc-2-clean-merged_20260822`.
- Focused on balanced spatial coverage and clean centered grasps instead of continuing to append corrections to the old distribution.

The resulting clean model achieved a reported **20/21 qualification result (95.2%)** during the August 22–23 qualification period.

## August 23 — Missing positions and final 450-episode training

- Recorded **72 additional demonstrations** at four gaps: between top-left/top, top/top-right, bottom-left/bottom, and bottom/bottom-right.
- Covered all six orientations with three repetitions at each added position.
- Confirmed **450 episodes** in `TripleB3/rc-2-final-450_20260823`: 25 positions × 6 orientations × 3 repetitions.
- Reported final ACT training from scratch with **100,000 steps, batch size 8, one NVIDIA L40S, image transforms disabled, and 10,000-step checkpoints**.
- The complete final configuration, policy repository, frame count, and exact evaluated checkpoint have not been retained in this repository.

The current collection ledger reports approximately 750 demonstrations across the project. That approximate cumulative figure is separate from the exact 450-episode final dataset.

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
- Chose **no license** for the repository.

## September 9 to 15 — Ramp and final demonstration work

- Began the dedicated final-ramp-video discussion and planned a short demonstration of the finished system.
- By the mid-September project review, the ramp had been designed, 3D-printed, and reported working in the finished setup. Its exact print/completion date was not retained.
- Prepared to show the demonstration workflow, autonomous full cycle, randomized cube starts, and measured result.

The working ramp is a completed physical addition. Its earlier concept date does not establish that the August 23 trial table was generated using the finished ramp.

## September 14 to 15 — Proposed next vision phase

- Reviewed the completed ACT milestone and considered explicit RGB cube localization, camera calibration/mapping, commanded grasp motion, and fault handling as a next phase.
- Discussed ROS 2 and depth-based 3D localization within that extension scope.
- No completed explicit 3D detector, camera-to-robot transform pipeline, or ROS 2 motion-control implementation was established by the ACT acceptance result.

## Late September — Final video and portfolio narrative

- Developed a **47-second** video sequence covering the goal, teleoperation, ACT trained on 450 demonstrations, failure-driven refinement, a real-time autonomous cycle, and the 96.7% result.
- Used a failed attempt followed by a successful pickup from the same starting position after demonstration refinement and retraining.
- Drafted the final LinkedIn post around the transition from programmed manufacturing automation to learning from demonstrations.
- On September 29, stated an intention to publish the following morning. Publication itself is not confirmed in the available project history.
- Compiled a condensed project reference and reconciled dataset versions, evaluation stages, and earlier-versus-final training claims.

## September 29 to October 1 — Documentation corrections

- Merged PR #2 on **September 29**, preserving the complete final acceptance-test CSV and its measured outcome.
- Rewrote [Policy Training](policy-training.md) and corrected the corresponding README claims to distinguish the final 450-episode policy from the earlier 300-episode V7 run.
- Updated the [Evaluation Summary](../results/evaluation-summary.md) with the development tests, differing conditions, qualification/final failure distinction, and CSV-backed final metrics.
- Expanded this timeline using the current repository and recovered project-chat history.

As of **October 1, 2026**, the repository remains private. The completed ACT result and working printed ramp are documented; public repository release and LinkedIn/video publication remain unconfirmed.
