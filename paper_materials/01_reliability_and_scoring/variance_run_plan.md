# Variance Run Plan

Goal: address non-determinism by repeating generation and reporting variance.

- Documents: 3 self-scored source documents
- Runs per document: 5
- Randomness policy: fixed temperature and prompt; vary random seed per run
- Output to collect per run:
  - Recall-oriented coverage score
  - Count of generated steps
  - Run metadata (model, seed, timestamp, config hash)
- Report:
  - Mean, standard deviation, min/max per document
  - Cross-document aggregate summary

## Status
- In progress: running 5 generations per document.
