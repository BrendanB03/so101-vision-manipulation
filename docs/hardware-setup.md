# Hardware Setup

## Components

| Component | Role |
| --- | --- |
| SO-101 leader arm | Human demonstration interface |
| SO-101 follower arm | Executed teleoperated demonstrations and autonomous rollouts |
| Intel RealSense D455 | Workspace visual observation |
| Red cube | Manipulated object |
| Blue bin | Placement target |
| Ubuntu workstation | Recording, dataset management, training, and rollout |

The recorded workstation configuration was an Intel Core i7-8700K, NVIDIA RTX 2080 Super, 16 GB RAM, and Ubuntu 24.04 LTS.

## Arm connectivity

During the working setup, the follower enumerated as `/dev/ttyACM0` and the leader as `/dev/ttyACM1`. These paths are historical observations, not portable configuration: USB enumeration can change after reconnecting devices or rebooting.

Both arms were calibrated before recording. The project used persistent LeRobot calibration identities for the leader and follower, but conflicting shorthand versions of those identifiers appear in the recovered notes. They are intentionally not presented as canonical values here.

Before recording or rollout, verify:

1. The leader and follower are mapped to the intended serial devices.
2. Both arms have valid calibration data.
3. Motor power and communication are stable.
4. The follower can reproduce leader motion without binding or unexpected joint direction.
5. The regular teleoperation process is not running at the same time as another tool that controls the follower.

## Camera and workspace

The Intel RealSense D455 was positioned to show the cube, blue bin, follower arm, grasp approach, and placement region. The working stream ran at 30 FPS; different resolution values appeared during setup checks, so the exact recording resolution should be read from dataset metadata rather than inferred from these notes.

Camera geometry was a major experimental variable. During development, the camera was accidentally bumped and then repositioned by eye. Even when the scene looked similar to a person, the mapping between image pixels and robot coordinates changed. Earlier and later demonstrations were therefore not perfectly comparable, and the shift likely contributed to a right-side grasp bias.

For a reproducible setup:

- Rigidly mount the camera.
- Mark or measure its pose relative to the robot base and table.
- Lock exposure and other image settings when practical.
- Confirm that the exact camera stream, resolution, FPS, and key names match between recording and inference.
- Treat a camera movement as a dataset-version boundary unless the pose can be restored accurately.

## Workspace layout

The qualified task was:

> Pick up the red cube and place it in the blue bin.

Training expanded to 25 cube-location markers across the reachable workspace, followed by demonstrations emphasizing gaps, right-side positions, and varied orientations.

A low 3D-printed boundary and removable ramp were designed as a concept for hands-off position and yaw randomization. The recovered history confirms the design and planned use, but does not confirm that the ramp was used for the final 30 trials. It is therefore documented as an evaluation concept, not as completed final-test hardware.

## Operational precautions

- Keep people, cables, and loose objects outside the arm's motion envelope.
- Begin with conservative motion and a clear path to remove motor power.
- Confirm the cube and bin are visible before every rollout.
- Stop immediately after unexpected contact, joint binding, or camera movement.
- Revalidate calibration and camera alignment after any hardware change.

