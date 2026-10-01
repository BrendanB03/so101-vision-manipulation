# Software Setup

The project used LeRobot and LeLab to take an SO-101 leader/follower system from calibration and teleoperation through demonstration recording, ACT training, and autonomous red-cube-to-blue-bin evaluation. The final model used **450 demonstrations** and achieved **29/30 complete first-attempt tasks (96.7%)**.

This document describes the recovered workflow. The exact final environment export and launch commands were not retained, so the historical configuration examples below should be checked against the installed software and deployed policy before reuse.

## Recorded software stack

| Software | Role in the project |
| --- | --- |
| Ubuntu 24.04 LTS | Operating system on the robot workstation |
| Conda environment `lerobot` | Isolated Python environment for the robot workflow |
| Hugging Face LeRobot | Motor setup, calibration, device discovery, teleoperation, recording, dataset editing, ACT training, and rollout |
| LeLab | Interface used during development for the LeRobot recording, training, and rollout workflow |
| Intel RealSense tools and dependencies | D455 discovery and RGB camera integration |
| PyTorch and CUDA | Policy training and GPU inference |
| Hugging Face Hub | Private demonstration datasets and policy artifacts |
| Git and GitHub | Documentation, version history, and review |

ROS 2 was installed or explored during the wider project, but its use in the final ACT control path is not established by the retained record. Explicit object detection, camera-to-robot coordinate conversion, and depth-based 3D localization were discussed as later work.

The demonstrated ACT workflow used camera observations and robot state to predict action chunks. The D455's depth capability does not establish that depth was an input to the final policy. No separate custom application source tree was preserved; the recovered implementation work primarily used existing tools and configuration flags.

## Environment and account checks

The working environment was named `lerobot`. Environment activation is:

```bash
conda activate lerobot
```

Hugging Face identity checks used during troubleshooting were:

```bash
hf auth whoami
hf auth login
```

Use `hf auth login` when authentication is needed, and verify the intended account before recording or uploading. Dataset and policy IDs use the `owner/repository` form. An early recording attempt failed because the dataset ID lacked its owner namespace.

Exact Python, LeRobot, LeLab, PyTorch, CUDA, and RealSense dependency versions were not frozen in the surviving record. This repository does not supply a verified installation recipe or dependency lockfile. Authentication tokens should remain outside version control.

## Robot setup and device discovery

The leader and follower each required motor configuration, calibration, and motion verification before recording. Historical serial assignments were `/dev/ttyACM0` for the follower and `/dev/ttyACM1` for the leader; these assignments can change after reconnecting devices.

Tools discussed or used during setup included `lerobot-setup-motors`, `lerobot-calibrate`, `lerobot-find-port`, `lerobot-find-cameras`, and `lerobot-teleoperate`. Their flags and configuration names depend on the installed LeRobot version; this list is not a complete command sequence.

Before a session, identify both arms, load their corresponding calibration identities, confirm leader-to-follower motion, and verify the camera view. Preserve the robot type, joint/action features, and calibration identities with the experiment configuration. See [Hardware Setup](hardware-setup.md).

## Camera configuration history

The recovered camera settings came from different stages of development:

| Historical context | Camera identity or observation key | RGB stream |
| --- | --- | --- |
| Early camera tests | Device index 4 | 640 × 480 at 30 FPS |
| Native RealSense teleoperation example | D455 serial `039222250348`; key `front` | 1280 × 720 at 30 FPS |
| Recovered deployed checkpoint configuration | Key `workspace_cam` | 640 × 480; complete stream settings were not recovered |

These entries are historical examples, not interchangeable final settings. Device indices are temporary discovery results. The complete camera configuration used for the final 450-episode model was not retained.

Recording and rollout must match the policy's camera observation key and expected image dimensions. Preserve stream timing and the physical camera pose as well: the policy sees different pixels if the camera moves, even when the software configuration is unchanged.

## Recording, curation, and merging

The workflow progressed through:

1. Calibrate the arms and verify teleoperation with the camera.
2. Record synchronized images, robot state, and actions while the leader demonstrates the complete task.
3. Inspect recordings and curate the source datasets.
4. Validate and merge compatible sources.
5. Train ACT, deploy a selected checkpoint, and record physical evaluation results.
6. Use observed misses to choose the next recording conditions.

`lerobot-edit-dataset` was used for dataset operations. A merge attempt exposed a distinction between an input repository selector and the new output repository argument: the recovered correction used `--new_repo_id` for the destination. Check the installed tool's help rather than copying flags across versions.

Another merge failed because referenced episode metadata was missing. A valid merge requires intact source files, compatible robot/action features, camera keys and image shapes, and compatible timing. Check the resulting episode count and inspect playback before training.

The final collection was a clean rebuild: **378 episodes = 21 positions × 6 orientations × 3 repetitions**, followed by **72 episodes = 4 additional positions × 6 orientations × 3 repetitions**. The final **450 episodes** excluded the earlier accumulated series. See [Dataset Collection](dataset-collection.md).

## Training and checkpoint context

The local workstation included an RTX 2080 Super and 16 GB RAM. The reported final ACT training used one NVIDIA L40S, 100,000 steps, batch size 8, image transforms disabled, training from scratch, and checkpoints every 10,000 steps.

The earlier V7 run used a 300-episode dataset and was interrupted around 73,000 steps. A resume was configured from the 70,000-step checkpoint. That recovery belongs to the earlier run; it does not describe the final 450-episode training.

The final learning rate, seed, complete architecture configuration, frame count, policy repository, and exact deployed checkpoint were not independently recovered. The detailed V7 log must not be substituted for the final configuration. See [Policy Training](policy-training.md).

## Rollout consistency and future reproduction

Before autonomous evaluation, confirm that the dataset, policy, and live robot agree on calibration identity, joint/action schema, observation keys, image dimensions, and task conditions. A command completing successfully does not establish compatibility or comparable camera geometry.

For a future rerun, retain:

- A working environment export and exact software revisions.
- Non-secret motor, calibration, camera, recording, training, and rollout configurations.
- Dataset revisions, source lineage, episode/frame counts, and merge validation.
- The full training log, selected checkpoint, and exported policy configuration.
- Evaluation conditions and row-level outcomes linked to that checkpoint.

The repository now preserves the [final 30-trial results](../results/final-test-results.csv), but it does not contain a complete reproducible runtime bundle. See [Inference and Evaluation](inference-and-evaluation.md) and the [Project Timeline](project-timeline.md) for the deployment context.
