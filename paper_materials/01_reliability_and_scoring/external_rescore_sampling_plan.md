# External Re-Score Sampling Plan

Goal: complement single-rater author scores with an external expert check.

## Sampling
- Sample size target: at least 20% of generated outputs across the 3 documents
- Sampling strategy: stratified by document and quality band (high/medium/low author score)

## Raters
- At least 2 independent domain experts
- Blind review of generation source details when possible

## Scoring
- Use the same rubric as author scoring
- Capture:
  - Per-output score
  - Binary accept/reject decision
  - Optional disagreement notes

## Analysis
- Report inter-rater agreement (e.g., Cohen's kappa for binary accept/reject)
- Compare external vs. author scores with mean delta and confidence interval
