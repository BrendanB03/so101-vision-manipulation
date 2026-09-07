# Policy Training

## Policy selection

Action Chunking Transformer (ACT) was selected as the first learned control policy. Instead of predicting only the next individual motor command, ACT predicts a sequence of actions, supporting smoother multi-step manipulation such as approach, grasp, lift, transport, and release.

This repository documents application and evaluation of ACT through LeRobot; it does not claim authorship of the ACT architecture or LeRobot implementation.

## Final recorded training configuration

| Parameter | Recorded value |
| --- | --- |
| Dataset | `TripleB3/red-cube-v7-rebuilt_20260816` (private) |
| Policy repository | `TripleB3/act_2026-08-16_15-24-21` (private) |
| Training steps | 100,000 |
| Effective batch size | 8 |
| Learning rate | `1e-5` |
| Vision backbone | ResNet-18 |
| Action chunk / action steps | 100 |
| Transformer model dimension | 512 |
| Feed-forward dimension | 3,200 |
| Attention heads | 8 |
| Variational encoder | Enabled |
| Image transforms | Disabled |
| Seed | 1000 |
| Dataset episodes | 300 |
| Dataset frames | 97,030 |
| Trainable parameters | 51,597,190 (approximately 51.6 million) |

The recovered log shows the run resuming from a 70,000-step checkpoint and completing the 100,000-step target. Checkpoint and Hub-save intervals were recorded at 10,000 steps for that run.

## Command provenance

The complete historical training command was not recoverable from the project conversations. The configuration above comes from the recorded run configuration and log, which is more reliable than reconstructing a command from memory.

No executable training command is presented as historical fact in this repository.

## Training workflow

1. Validate the rebuilt dataset and observation schema.
2. Configure ACT with the recorded camera and robot features.
3. Train on the fixed offline demonstration dataset.
4. Save intermediate checkpoints.
5. Resume from the last valid checkpoint when a run is interrupted.
6. Evaluate rollouts in the real workspace.
7. Use observed failures to plan the next data-collection stage.

## Checkpoint selection

The final project qualification was based on real-world task performance, not training loss alone. The retained policy had to complete the full red-cube-to-blue-bin behavior across held-out position and orientation combinations.

## Compute notes

The local workstation included an NVIDIA RTX 2080 Super. Project records also describe cloud GPU training experiments, including an L40S configuration and a GPU job that ended in error. The exact relationship between every cloud attempt and the final qualified checkpoint was not fully recoverable, so this repository does not attribute the final model to a specific cloud job.

## What should be retained in a future run

- Exact command line
- Environment export and LeRobot commit
- Dataset commit/revision
- Full policy configuration
- Checkpoint hashes
- Training and evaluation logs
- Explicit reason for selecting the production checkpoint

