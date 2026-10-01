# Troubleshooting and Lessons Learned

The project developed from a repeatable fixed-position demonstration into a policy that completed **29/30 randomized pickup-and-bin trials (96.7%)** on its first attempt. Progress came from finding specific weaknesses in the recording and evaluation workflow, then changing the data collected.

The lessons below distinguish observed failures and completed changes from practices recommended for future runs.

## 1. Verify account identity and repository naming before recording

An early recording attempt failed before producing usable episodes because the Hugging Face dataset ID omitted the required user namespace.

The recorded identity and authentication commands were:

```bash
hf auth whoami
hf auth login
```

Use an `owner/repository` ID and verify the intended account before starting a long session. Authentication and repository naming are separate checks; a valid login does not repair an incorrectly formed ID.

## 2. Match dataset commands to the installed tool version

A dataset-editing attempt used the wrong argument for the output repository. The recovered correction used `--new_repo_id` to name the destination. An input selector such as `--repo_id` can serve a different purpose depending on the operation.

The practical lesson is to check the installed command's help and retain the exact successful command alongside the software revision. A remembered command from another release is not sufficient evidence of a reproducible workflow.

## 3. Validate source contents before merging

A merged dataset referenced `meta/episodes/chunk-000/file-001.parquet`, but that file was absent. The problem was incomplete dataset metadata, not repository privacy.

The response was to rebuild from intact source datasets rather than repeatedly reuse the broken merge. Source preservation made that recovery possible.

Before training a merged dataset, check that referenced files exist, episode counts agree with the intended sources, robot and camera features are compatible, and representative episodes play back correctly. A repository being accessible does not establish that its contents are complete.

## 4. Camera pose is part of the learned task

The D455 was bumped and repositioned by eye during development. This changed the image-to-workspace relationship and reduced comparability between earlier and later demonstrations.

Camera movement likely contributed to directional grasp bias, but the project did not isolate it in a controlled causal experiment. Coverage and demonstration quality were also changing.

For future collection, rigidly mount the camera, record its pose, and capture a reference view. Treat movement as a changed experimental condition and recheck policy behavior before mixing recordings. Matching a camera key and resolution alone cannot restore the original view.

## 5. Stable device and observation identities matter

Historical camera examples used different observation keys and stream dimensions: `front` at 1280 × 720 in one teleoperation example and `workspace_cam` at 640 × 480 in a recovered checkpoint configuration. Serial ports and camera indices were also specific to the connected setup.

Use discovery to verify devices after reconnecting them, then compare the live robot and camera configuration with the policy's expected features. Preserve calibration identities and observation names with each experiment. The complete final camera configuration was not retained, so these historical examples should not be combined into an assumed final setup.

## 6. Repeatability, position coverage, and orientation coverage require separate tests

Different evaluations revealed different limitations:

| Diagnostic | Result | What it exposed |
| --- | ---: | --- |
| Fixed-position baseline | 10/10 | Repeatability at the taught position and orientation |
| V2 position test | 24/30 (80.0%) | Uneven spatial performance; the right side succeeded in 1/5 trials |
| V5 position qualification | 29/30 (96.7%) | Right-side performance reached 5/5, while right-biased grasps remained |
| V6 center-orientation test | 14/20 (70.0%) | Orientation weakness, especially at +75° and −15° |
| Separate V6 randomized position-and-orientation test | 14/30 pickups (46.7%) | Combined variation exposed lateral misses, top contacts, and orientation failures |

A separate fixed-center retest achieved 10/10 pickups despite persistent grasp bias. Successful center behavior therefore did not establish reliable behavior across positions and orientations.

A broader diagnostic was also reported at approximately 11/20 (55.0%) before the clean rebuild. Its exact checkpoint and conditions were not recovered; it is a separate result from the 14/30 pickup test.

Targeted right-side recordings improved a known spatial weakness. Subsequent orientation testing showed why both dimensions needed deliberate coverage rather than relying on a strong position-only score.

## 7. More episodes did not automatically remove grasp bias

The earlier dataset series grew through targeted additions and merged snapshots of 240, 300, and 360 episodes. Orientation and centered-grasp corrections were added, but repeated use of older recordings retained the earlier distribution.

The later reset excluded that accumulated series and collected a fresh, balanced set:

| Collection | Structure | Episodes |
| --- | --- | ---: |
| Clean rebuild | 21 positions × 6 orientations × 3 repetitions | 378 |
| Four missing position gaps | 4 positions × 6 orientations × 3 repetitions | 72 |
| Final collection | 25 positions × 6 orientations × 3 repetitions | 450 |

The six taught angles were 0°, 15°, 30°, 45°, 60°, and 75°. Collection emphasized clean centered grasps and consistent coverage.

The 378-episode model qualified at 20/21 (95.2%). After filling the four top/bottom gaps, the 450-episode model qualified at 24/25 (96.0%); all four new locations succeeded, and the sole failed trial struck the cube's top.

The lesson is to inspect what a dataset teaches, including its repeated biases. A fresh balanced collection can be more useful than continually appending corrections. These tests support the value of the combined refinement process, but do not isolate the contribution of each change.

## 8. Checkpoints enable recovery, but training claims need their own evidence

The earlier 300-episode V7 training run was interrupted around 73,000 steps. A 70,000-step checkpoint was available, and a resumed run was configured with a 100,000-step target. The surviving startup log does not prove that the resumed run reached that target.

Final 450-episode training was separately reported as starting from scratch, with a 100,000-step target and 10,000-step checkpoints.

Retain checkpoint directories, complete logs, launch configuration, and the exact checkpoint used for evaluation. A target step count, a resume command, and a completed training run are different pieces of evidence.

## 9. Define success before counting trials

The final acceptance test allowed no retries and counted complete autonomous pickup-and-bin placement. It achieved **29/30 (96.7%)** across randomized intermediate positions and cube angles from **4° to 86°**.

The sole failure was trial 15 at 63°, recorded as “Failed to orient properly.” Trial 30 contacted the cube's top but still completed successfully; this was distinct from the top-contact failure in the earlier 24/25 qualification.

Successful trials also included notes about biased grasps. The task success rate therefore does not measure centered grasp quality or contact-free execution.

The final record did not separately score pickup and placement. Earlier pickup-only tests should retain that label. Development tests used different conditions and denominators, so their percentages are milestones rather than a controlled comparison under one protocol.

The supported result is strong performance within this tested single-cube tabletop setup. New objects, camera poses, lighting, and wider workspace conditions require their own evaluation.

## 10. Preserve lineage and evidence while experiments are running

The current dataset ledger reports approximately **750 demonstrations collected or curated across the project**. That is distinct from the **450 episodes used by the final model**. Merged snapshots reuse recordings, so their episode counts cannot be summed to calculate unique collection totals.

Earlier partial summaries confused intermediate datasets with the final training set. The detailed 300-episode V7 configuration is useful historical evidence, but it does not supply the final model's unverified learning rate, architecture, seed, frame count, or checkpoint identity.

For each experiment, preserve the dataset revision and source lineage, camera configuration, validation checks, training configuration, selected policy checkpoint, and evaluation conditions together.

The repository now includes the [complete final trial CSV](../results/final-test-results.csv). A complete frozen environment, final launch commands, and all earlier trial records are still missing. Keeping the repository current during collection would make future comparisons and reproduction substantially easier.

## Skills developed

The work improved practical skills in Linux environments, serial-device discovery, motor configuration and calibration, leader-follower teleoperation, RGB camera integration, demonstration curation, dataset repair, ACT training and checkpoint recovery, and physical evaluation.

Failure analysis became more specific: workspace region, cube yaw, grasp alignment, camera geometry, and complete-task outcome were examined separately. The later designed and printed working ramp added experience in physical fixture design for variable cube starts; its later completion does not establish that it was used in the August acceptance test.

See [Software Setup](software-setup.md), [Dataset Collection](dataset-collection.md), [Policy Training](policy-training.md), [Evaluation Summary](../results/evaluation-summary.md), and [Project Timeline](project-timeline.md) for the supporting history.
