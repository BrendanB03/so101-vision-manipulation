# Dataset Collection

## Demonstration task

Leader-follower teleoperation was used to record the complete behavior:

> Pick up the red cube and place it in the blue bin.

Each useful episode needed a visible cube and bin, a clean approach, a stable grasp, successful transport, and release into the bin. Failed or interrupted demonstrations were deleted; curation quality directly affected policy behavior.

## Collection strategy

The dataset evolved in response to evaluation failures:

1. **Controlled demonstrations** established the end-to-end pipeline.
2. **Varied-position demonstrations** expanded the reachable pickup region.
3. **Targeted right-side and intermediate-position demonstrations** addressed weak regions and directional grasp bias.
4. **Orientation-focused and corrective demonstrations** addressed angle-dependent misses and off-center grasps.
5. **A clean balanced rebuild** replaced the accumulated series with 21 positions × 6 orientations × 3 repetitions.
6. **Four missing-position recordings** expanded the clean dataset to 25 positions; compatible sources were merged into the final 450 episodes.

Approximately 750 teleoperated demonstrations were reported as collected and curated across the project. This cumulative estimate is distinct from the 450 episodes used to train the final policy.

## Dataset History

| Stage | Episode count | Description |
| --- | --- | --- |
| Initial recording test | 5 | Basic red-cube-to-blue-bin demonstrations to verify teleoperation, camera recording, and dataset creation. |
| Fixed-position baseline | 25 | All demonstrations used the same cube position and orientation, teaching a consistent pickup, transport, and placement sequence. |
| V2 position variation | 80 | Varied cube positions while keeping orientation constant. Testing exposed weak performance on the right side of the workspace. |
| V3 position correction | 100 | Added position coverage, particularly around the weak right side and nearby intermediate positions. |
| V5 position merge | 180 | Combined V2 and V3. Position performance improved, but right-biased grasps remained. |
| V6 orientation recordings | 60 | Introduced cube orientation variation to expand beyond the earlier position-focused demonstrations. |
| V6 combined dataset | 240 | Combined V2’s 80, V3’s 100, and 60 orientation demonstrations. |
| V7 corrective recordings | 60 | Targeted orientation-dependent failures and off-center grasps, emphasizing difficult angles and right-side workspace regions. |
| V7 rebuilt dataset | 300 | Combined the 240-episode base with the 60 V7 corrective demonstrations. |
| Post-V7 corrective recordings | 60 | Targeted persistent rightward grasp bias through centered-grasp demonstrations across positions and orientations. The source was archived as `TripleB3/red-cube-v9_20260816_151902`. |
| V8 combined dataset | 360 | Combined the five original source datasets: 80 + 100 + 60 + 60 + 60. Earlier records also called this merge V9. |
| Clean orientation rebuild | 63 per angle; 378 combined | Fresh recordings covering 21 positions at 0°, 15°, 30°, 45°, 60°, and 75°, with three repetitions per combination. Excluded the old dataset series. |
| Missing-position recordings | 72 | Added four gaps: top-left/top, top/top-right, bottom-left/bottom, and bottom/bottom-right. Covered all six orientations with three repetitions each. |
| Final training dataset | **450** | `TripleB3/rc-2-final-450_20260823`: 378 clean demonstrations + 72 additional recordings = **25 positions × 6 orientations × 3 repetitions**. Used for the final reported **29/30 successful first-attempt complete tasks**, with no retries. |

## Compatibility checks before a merge

- Robot and action feature schemas match.
- Camera observation keys and image shapes match.
- FPS and episode timing are compatible.
- All episode parquet files referenced by metadata exist.
- Episode counts agree across metadata and physical files.
- Random samples can be visualized and decoded.
- Source repositories are retained until the merged result is validated.

## Bias and coverage lessons

More demonstrations did not automatically produce better generalization. Distribution mattered:

- Dense center coverage did not guarantee success at workspace edges.
- A camera pose shift made old and new image distributions less comparable.
- Targeted right-side examples improved regional performance, but off-center grasps remained; coverage and camera geometry were both relevant.
- Position coverage alone did not solve orientation failures.
- Targeted examples based on observed failures were more useful than undirected repetition.
