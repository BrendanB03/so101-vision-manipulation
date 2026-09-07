# Software Setup

## Recorded stack

| Software | Project role |
| --- | --- |
| Ubuntu 24.04 LTS | Robot workstation operating system |
| Conda environment `lerobot` | Isolated project environment |
| Hugging Face LeRobot | Robot configuration, teleoperation, recording, dataset tooling, training, and rollout |
| PyTorch | ACT policy training and inference backend |
| CUDA | NVIDIA GPU acceleration |
| Hugging Face Hub | Private dataset and policy versioning |
| Git and GitHub | Portfolio documentation and change review |

No custom application source code was preserved for this project. The work was performed mainly through LeRobot command-line workflows and configuration flags.

## Reproducibility status

The original project history does not contain a complete environment export or a single verified installation command sequence. This repository therefore does not present a guessed package lockfile or fabricated setup commands.

Before reproducing the work, capture the following for the current LeRobot version:

- Python version
- LeRobot commit or release
- PyTorch and CUDA versions
- RealSense dependencies
- SO-101 robot configuration names
- Dataset feature schema
- Camera key, resolution, and FPS
- Policy configuration

## Activation and identity checks

The project used a Conda environment named `lerobot`. Hugging Face authentication was checked during dataset troubleshooting with:

```bash
hf auth whoami
```

When the account was not authenticated, the recorded corrective command was:

```bash
hf auth login
```

Credentials and tokens must never be committed to this repository.

## Configuration consistency

Recording, training, and rollout must agree on:

- Follower robot type and calibration identity
- Camera names and observation keys
- Image dimensions and FPS
- Joint/action feature schema
- Dataset task description
- Policy input and output features

The project showed that a pipeline can execute without being experimentally comparable. A camera pose change or schema mismatch can invalidate assumptions even when individual commands succeed.

## Recommended environment capture

For a future rerun, save a reviewed environment record after the system works:

1. Export the Conda environment.
2. Record the LeRobot Git commit.
3. Record GPU and CUDA information.
4. Save non-secret robot and camera configuration.
5. Add a dataset card and model card.
6. Tag the Git commit used for each experiment.

