# Policy Training

The final ACT policy learned to pick up a red cube and place it in a blue bin from varied positions and orientations. It was trained on a **450-episode demonstration dataset** and achieved **29 successful first-attempt trials out of 30 (96.7%)** in the final randomized evaluation.

This page describes the reported final training setup and the separately recorded configuration of an earlier 300-episode V7 experiment.

## Learning from demonstrations

Leader-follower teleoperation supplied successful examples of approach, grasp, lift, transport, and release. LeRobot recorded camera observations, robot state, and actions for offline imitation learning.

Action Chunking Transformer (ACT) predicts a sequence of actions from visual observations and robot state. The project applied ACT through LeRobot and tested the resulting behavior on the physical SO-101 follower arm.

## Final training dataset

The final dataset was `TripleB3/rc-2-final-450_20260823`, comprising:

- **378 clean demonstrations:** 21 positions × 6 orientations × 3 repetitions.
- **72 additional demonstrations:** 4 missing positions × 6 orientations × 3 repetitions.
- **450 total demonstrations:** 25 positions × 6 orientations × 3 repetitions.

The six recorded orientations were 0°, 15°, 30°, 45°, 60°, and 75°. The clean recording series replaced the earlier accumulated datasets. Its focus was balanced coverage and consistent centered grasps after previous experiments exposed position gaps and rightward grasp bias.

See [Dataset Collection](dataset-collection.md) for the collection history.

## Reported final training setup

| Parameter | Reported value |
| --- | --- |
| Dataset | `TripleB3/rc-2-final-450_20260823` |
| Dataset episodes | 450 |
| Policy | ACT |
| Training steps | 100,000 |
| Batch size | 8 |
| Initialization | From scratch; resume disabled |
| Training GPU | 1× NVIDIA L40S |
| Image transforms | Disabled |
| Checkpoint interval | 10,000 steps |

These settings were reported for the final training stage. The complete final training log, launch command, policy repository ID, and exact deployed checkpoint have not been retained in this repository. The final frame count, full architecture configuration, seed, learning rate, and parameter count therefore remain unverified here.

## Training and physical validation

Dataset changes followed observed rollout failures. Position-focused recordings improved workspace coverage; orientation recordings and corrective demonstrations addressed angle-dependent misses and off-center grasps. The final clean rebuild made the position-and-orientation distribution deliberate and balanced.

The clean 378-episode model achieved a reported 20/21 qualification result. After adding the missing-position recordings, the 450-episode model achieved 24/25 in qualification and **29/30 in the final randomized acceptance test**. These tests used different conditions and sample sizes.

Qualification depended on completing the physical task, rather than training loss alone. The final test counted autonomous pickup and placement on the first attempt, with no retries. Its single failure occurred at 63° and was recorded as a failure to orient properly.

See [Inference and Evaluation](inference-and-evaluation.md) and the [complete final trial record](../results/final-test-results.csv).

## Earlier V7 training configuration

The detailed configuration below belongs to the **August 16, 2026 V7 experiment**, which used 300 episodes. It is preserved as experiment history and does not establish the configuration of the final 450-episode policy.

| Parameter | Recorded V7 value |
| --- | --- |
| Dataset | `TripleB3/red-cube-v7-rebuilt_20260816` |
| Policy repository | `TripleB3/act_2026-08-16_15-24-21` |
| Training step target | 100,000 |
| Effective batch size | 8 |
| Learning rate | `1e-5` |
| Vision backbone | ResNet-18 |
| Action chunk size | 100 |
| Action steps | 100 |
| Transformer model dimension | 512 |
| Feed-forward dimension | 3,200 |
| Attention heads | 8 |
| Variational encoder | Enabled |
| Image transforms | Disabled |
| Seed | 1000 |
| Dataset episodes | 300 |
| Dataset frames | 97,030 |
| Trainable parameters | 51,597,190 |
| Checkpoint interval | 10,000 steps |

The recovered log shows V7 resuming from checkpoint `070000` with a 100,000-step target. The saved excerpt contains the resumed training startup, not a completion record. That recovery belongs to V7; the reported final 450-episode run started from scratch.

## Reproduction and experiment records

The repository documents the historical workflow. The private demonstrations, model weights, calibration files, and frozen environment needed for exact reproduction are not included.

For a reproducible training run, retain:

- The exact training command and LeRobot revision.
- The dataset revision, episode count, frame count, and included sources.
- Camera observation keys, image dimensions, and workspace geometry.
- The complete policy and training configuration.
- Training logs, checkpoint identity, and the checkpoint used for evaluation.
- Physical trial results and the reason for selecting the evaluated checkpoint.
