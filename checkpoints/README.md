# Checkpoint Policy

This folder documents checkpoint handling for reproducibility.

## Recommended practice

- keep only lightweight or shareable checkpoints in the public repository,
- describe the training lineage of each public checkpoint,
- do not upload very large checkpoints if they violate storage limits or sharing constraints.

## Suggested naming

- `latest.pt`
- `best.pt`
- `latest_step.pt`
- `epoch_XXX.pt`

## What to document for each released checkpoint

- config file used,
- training stage,
- resume source,
- epoch or step number,
- intended evaluation command,
- known limitations.
