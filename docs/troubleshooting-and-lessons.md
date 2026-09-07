# Troubleshooting and Lessons Learned

## 1. Hugging Face repository identity

An early recording attempt failed before producing usable episodes because the dataset repository ID did not include the required user namespace.

Useful identity checks recorded during the project were:

```bash
hf auth whoami
hf auth login
```

The lesson was to verify authentication and use an `owner/repository` ID before starting a long recording session.

## 2. Missing episode metadata during merge

A merged dataset referenced `meta/episodes/chunk-000/file-001.parquet`, but that file was absent from the repository. The merge could not treat the dataset as complete.

The failure was not attributed to repository privacy. The safe response was to validate source contents and rebuild the training dataset rather than repeatedly merge a broken version.

## 3. Camera movement changed the problem

The D455 was bumped and repositioned by eye. This introduced a change in camera-to-workspace geometry that was visually subtle but important to the policy.

Consequences included:

- Reduced comparability between older and newer demonstrations
- Different pixel locations for the same robot-space position
- A likely contribution to right-side grasp bias
- Additional uncertainty when merging datasets from different camera poses

The corrective lesson is to rigidly mount and measure the camera, then treat any movement as a new experimental condition.

## 4. Controlled success did not equal generalization

Early controlled checks were successful, but the initial generalized evaluation reached only 11/20. The policy had learned the demonstration distribution, not a universal pick-and-place rule.

Generalization improved by deliberately filling spatial and orientation gaps, not by repeating already successful center-position demonstrations.

## 5. Position and orientation are separate coverage dimensions

The position-focused dataset improved spatial coverage, while orientation remained weak. Later demonstrations varied cube yaw and included intermediate orientations. This separation made failure analysis more actionable.

## 6. Dataset counts need provenance

The project referenced 80-, 100-, 125-, 175-, 240-, and 300-episode datasets, plus approximately 450 cumulative demonstrations. Without labels, those numbers appear contradictory.

Each future experiment should record:

- Dataset repository and revision
- Episode and frame counts
- Camera configuration
- Collection objective
- Included source datasets
- Validation status
- Policy trained from that dataset

## 7. First-attempt evaluation prevents inflated results

The final acceptance test counted only the first autonomous attempt and allowed no retries. This made 29/30 a meaningful task-level result rather than a best-of-several result.

## 8. Preserve the evidence while working

The project was completed mainly through terminal commands, but the full command history, environment export, row-level final table, and media were not preserved in the repository. Future projects should create the repository at the beginning and commit documentation alongside experiments.

