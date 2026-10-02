# Hardware Setup

## Components

| Component | Role |
| --- | --- |
| SO-101 leader arm | Human demonstration interface |
| SO-101 follower arm | Executed teleoperated demonstrations and autonomous rollouts |
| Intel RealSense D455 | RGB workspace observations for the learned policy |
| Red cube | Manipulated object |
| Blue bin | Placement target |
| Ubuntu workstation | Local recording, dataset management, early training, and rollout |

The recorded local workstation configuration was an Intel Core i7-8700K, NVIDIA RTX 2080 Super, 16 GB RAM, and Ubuntu 24.04 LTS. Final 450-episode ACT training was on one NVIDIA L40S; see [Policy Training](policy-training.md).

## Arm connectivity

Historical serial assignments were `/dev/ttyACM0` for the follower and `/dev/ttyACM1` for the leader. These assignments can change after reconnecting devices.

Both arms were calibrated before recording. The project used persistent LeRobot calibration identities for the leader and follower.

Before recording or rollout, verify:

1. The leader and follower are mapped to the intended serial devices.
2. Both arms have valid calibration data.
3. Motor power and communication are stable.
4. The follower can reproduce leader motion without binding or unexpected joint direction.
5. The regular teleoperation process is not running at the same time as another tool that controls the follower.

## Camera and workspace

The Intel RealSense D455 provided an RGB view of the cube, blue bin, gripper approach, and placement region. Historical RGB stream examples ran at 30 FPS, but the complete final camera configuration was not retained. See [Software Setup](software-setup.md) for the distinct historical observation keys and image dimensions. 

For a reproducible setup:

- Rigidly mount the camera.
- Mark or measure its pose relative to the robot base and table.
- Lock exposure and other image settings when practical.
- Confirm that the exact camera stream, resolution, FPS, and key names match between recording and inference.
- Treat a camera movement as a dataset-version boundary unless the pose can be restored accurately.

## Workspace layout

The qualified task was:

> Pick up the red cube and place it in the blue bin.

Earlier recordings targeted weak right-side regions and orientation failures. The final clean collection covered 21 positions at six orientations with three repetitions, then added four top/bottom gap positions. This produced 450 demonstrations across 25 training locations.

A low workspace boundary and removable ramp were initially proposed for position and yaw randomization. The ramp was subsequently designed, 3D-printed, and utilized in the finished setup by mid-September.

## Operational precautions

- Keep people, cables, and loose objects outside the arm's motion envelope.
- Begin with conservative motion and a clear path to remove motor power.
- Confirm the cube and bin are visible before every rollout.
- Stop immediately after unexpected contact, joint binding, or camera movement.
- Revalidate calibration and camera alignment after any hardware change.

