# Dataset Collection

## Demonstration task

Leader-follower teleoperation was used to record the complete behavior:

> Pick up the red cube and place it in the blue bin.

Each useful episode needed a visible cube and bin, a clean approach, a stable grasp, successful transport, and release into the bin. Failed or interrupted demonstrations were not useful simply because they existed; curation quality directly affected policy behavior.

## Collection strategy

The dataset evolved in response to evaluation failures:

1. **Controlled demonstrations** established the end-to-end pipeline.
2. **Varied-position demonstrations** expanded the reachable pickup region.
3. **Twenty-five training locations** imposed deliberate spatial coverage.
4. **Gap and right-side demonstrations** addressed sparse areas and directional bias.
5. **Orientation-focused demonstrations** expanded cube yaw coverage.
6. **Merged and rebuilt datasets** consolidated compatible episodes for later ACT training.

Approximately 450 teleoperated demonstrations were collected and curated across the project. This cumulative total includes successive data-collection stages; it must not be confused with the episode count used by one training run.

## Reconciled dataset history

| Stage | Episode count | What is verified |
| --- | ---: | --- |
| Early version 2 | 80 | Project history explicitly reported 80 episodes. Exact repository ID was not recovered. |
| Early version 3 | 100 | Project history explicitly reported 100 episodes. Exact repository ID was not recovered. |
| Original accumulated dataset | 125 | Referenced as the starting point for a position-expansion stage; exact repository ID was not recovered. |
| Position-expanded stage | 175 | Referenced as the position-generalization model dataset; exact repository ID was not recovered. |
| Merged version 6 | 240 | Private archival ID: `TripleB3/red-cube-v6-combined_20260815`. |
| Rebuilt version 7 | 300 | Private archival ID: `TripleB3/red-cube-v7-rebuilt_20260816`; the recorded training log reported 97,030 frames. |
| Cumulative project collection | approximately 450 | Total demonstrations gathered/curated over the project, not a single dataset snapshot. |

These counts are version snapshots and are not additive.

## Merge integrity issue

A merge involving the private archival dataset `TripleB3/red-cube-v5-merged_20260811` failed because a referenced episode metadata file, `meta/episodes/chunk-000/file-001.parquet`, could not be found.

The repository's privacy setting was not established as the cause. The issue was treated as a dataset-integrity or repository-content problem. A later rebuilt dataset was used instead of presenting the incomplete version as valid.

The beginning of the version 6 merge command was preserved, but the full list of input repository IDs was truncated in the available history. Because an executable command cannot be verified, it is not reconstructed here.

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
- Sparse right-side examples contributed to directional grasp behavior.
- Position coverage alone did not solve orientation failures.
- Targeted examples based on observed failures were more useful than undirected repetition.

## Data privacy

The Hugging Face datasets remain private. This repository records private archival identifiers for provenance but does not treat them as public reproduction links. No dataset files, parquet data, camera frames, or authenticated download URLs are committed here.

