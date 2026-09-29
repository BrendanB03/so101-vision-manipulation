# Autonomous Vision-Guided Pick-and-Place with SO-101

An end-to-end robotics and imitation-learning project in which an SO-101 follower arm learned to pick up a red cube and place it in a blue bin from varied workspace positions and orientations. The system used leader-follower demonstrations, an Intel RealSense D455 camera, Hugging Face LeRobot, and an Action Chunking Transformer (ACT) policy.

> **Final result:** 29 of 30 successful first-attempt trials (**96.7%**) across randomized cube positions and orientations. The evaluation included intermediate positions between the 25 training locations and angles from 4 degrees to 86 degrees.

> **Demonstration video:** Pending. A short uncut final-test video and a concise project overview will be added after media review.

## Project objective

The goal was to build a complete learning-from-demonstration workflow rather than program a fixed robot trajectory:

1. Assemble and calibrate SO-101 leader and follower arms.
2. Teleoperate the follower with the leader arm.
3. Record synchronized camera observations, robot state, and actions.
4. Train an ACT visuomotor policy with LeRobot.
5. Diagnose failure patterns and redesign the dataset.
6. Validate autonomous behavior at randomized positions and orientations.

The original project plan considered a separate OpenCV object-detection stage. The completed implementation instead used ACT as the visuomotor controller; no standalone OpenCV detector is claimed in this repository.

## System overview

| Layer | Implementation |
| --- | --- |
| Robot | SO-101 leader and follower arms |
| Visual input | Intel RealSense D455 |
| Demonstration interface | Leader-follower teleoperation |
| Learning framework | Hugging Face LeRobot with PyTorch |
| Policy | Action Chunking Transformer (ACT) |
| Compute | Ubuntu 24.04 workstation, Intel Core i7-8700K, NVIDIA RTX 2080 Super, 16 GB RAM |
| Task | Pick up a red cube and place it in a blue bin |

The camera observed the workspace while the policy used visual observations and robot state to predict chunks of follower-arm actions. The learned behavior covered approach, grasp, lift, transport, and release.

## Engineering progression

The main challenge was not achieving one successful pick; it was obtaining reliable generalization away from heavily represented training conditions.

| Stage | Recorded result | What it showed |
| --- | ---: | --- |
| Early controlled check | 10/10 | The basic recording, training, and rollout pipeline worked under narrow conditions. |
| Initial generalized evaluation | 11/20 (55.0%) | Position and orientation coverage was insufficient. |
| Intermediate qualification | 20/21 (95.2%) | Targeted data collection produced a large improvement; the exact test breakdown was not retained in the recovered notes. |
| Expanded-position qualification | 24/25 (96.0%) | Four added top/bottom gap positions succeeded; the lone failure struck the top of the cube. |
| Final randomized acceptance test | 29/30 (96.7%) | The policy generalized across randomized intermediate positions and orientations on first attempts. |

Approximately 450 teleoperated demonstrations were collected and curated across the project. This is a cumulative project figure, not the size of one training dataset. The final recorded ACT training run used a rebuilt 300-episode dataset containing 97,030 frames and trained to 100,000 steps.

## Dataset development

Dataset changes were driven by observed failures rather than by simply increasing episode count. The project progressed through controlled demonstrations, varied-position data, targeted right-side coverage, orientation-focused examples, merged datasets, and a rebuilt final dataset.

Several episode counts appear in the project history—80, 100, 125, 175, 240, and 300. They are version snapshots and are **not additive**. See [Dataset Collection](docs/dataset-collection.md) for the reconciled history and privacy notes.

## ACT training

The final recorded training configuration used:

- 100,000 training steps
- Batch size 8
- ResNet-18 vision backbone
- 100-step action chunks
- 512-dimensional transformer model
- 3,200-dimensional feed-forward layer
- 8 attention heads
- Variational encoder enabled
- Learning rate of `1e-5`
- Approximately 51.6 million trainable parameters

The run was resumed from the 70,000-step checkpoint and completed at 100,000 steps. Full recovered configuration details and provenance are documented in [Policy Training](docs/policy-training.md).

## Final evaluation

The final acceptance test used 30 first-attempt trials with no retries. A trial counted as successful only when the robot autonomously grasped the cube and placed it in the blue bin.

| Metric | Result |
| --- | ---: |
| Trials | 30 |
| Successful complete tasks | 29 |
| Failed complete tasks | 1 |
| Success rate | 96.7% |
| Tested angle range | 4–86 degrees |
| Retries | 0 |
| Only failure | Trial 15 at 63 degrees; the policy failed to orient properly |

The complete trial-by-trial record is available in [Final Test Results](results/final-test-results.csv). The source table recorded position, angle, overall success, and notes; it did not separately score grasp and bin-placement outcomes.

## Problems solved

- **Position bias:** Early policies performed well near familiar locations but degraded elsewhere. Demonstrations were redistributed across the workspace.
- **Orientation bias:** Cube yaw was underrepresented. Orientation-focused demonstrations and intermediate angles were added.
- **Right-side grasp bias:** The camera was accidentally bumped and repositioned by eye, changing camera-to-workspace geometry. This reduced comparability with earlier data and likely contributed to a directional bias.
- **Dataset integrity:** A merged dataset referenced a missing episode metadata parquet file. The final training dataset was rebuilt and validated before use.
- **Evaluation discipline:** The final test used first attempts only and no retries, preventing successful reruns from inflating the result.

## Repository guide

- [Hardware Setup](docs/hardware-setup.md)
- [Software Setup](docs/software-setup.md)
- [Dataset Collection](docs/dataset-collection.md)
- [Policy Training](docs/policy-training.md)
- [Inference and Evaluation](docs/inference-and-evaluation.md)
- [Troubleshooting and Lessons](docs/troubleshooting-and-lessons.md)
- [Project Timeline](docs/project-timeline.md)
- [Evaluation Summary](results/evaluation-summary.md)
- [Media Plan](media/README.md)

## Reproduction overview

This repository documents the historical workflow but does not contain the private demonstrations, model checkpoints, machine-specific calibration files, or a frozen environment export.

To reproduce the project:

1. Install and calibrate compatible SO-101 leader and follower arms with LeRobot.
2. Configure a fixed workspace camera and verify the exact stream used for recording.
3. Record clean demonstrations using consistent task wording, geometry, and episode schema.
4. Inspect dataset metadata and episode playback before merging versions.
5. Train an ACT policy, retaining configuration and checkpoint provenance.
6. Evaluate on untouched first-attempt trials across held-out positions and orientations.

Exact historical commands are included only where they were recoverable. Procedures are described conceptually where the original command text was unavailable.

## Limitations

- The final evaluation contained 30 trials in one controlled tabletop setup.
- Only one object-and-destination task was qualified: red cube to blue bin.
- Lighting, camera placement, bin position, and background variation were limited.
- The final policy was sensitive to camera-to-workspace geometry.
- Demonstration videos are not yet in the repository.
- Dataset and policy repositories remain private and are not public reproduction dependencies.

## Future improvements

- Run larger repeated evaluations with separate position, grasp, placement, and overall metrics.
- Test lighting, background, camera, bin, and object variation.
- Add additional objects and tasks.
- Compare ACT against other LeRobot policies using the same held-out test design.
- Preserve camera extrinsics and dataset manifests for every experiment.
- Publish a reviewed demonstration subset and model card if privacy and storage decisions permit.

## Acknowledgments

This project was built with [Hugging Face LeRobot](https://github.com/huggingface/lerobot), PyTorch, the SO-101 leader/follower platform, and an Intel RealSense D455 camera. ACT and LeRobot are upstream technologies; this repository documents their application, dataset development, integration, troubleshooting, and experimental validation in this project.
