# Precision Scoring Protocol

Goal: measure precision by counting erroneous and redundant steps.

## Step-level labels
- **Correct**: valid and necessary step for the target test sequence
- **Erroneous**: technically wrong, unsafe, or irrelevant step
- **Redundant**: duplicated or unnecessary step with no added value

## Precision metrics
- `precision_valid = correct_steps / total_generated_steps`
- `error_rate = erroneous_steps / total_generated_steps`
- `redundancy_rate = redundant_steps / total_generated_steps`

## Annotation workflow
1. Segment each generated sequence into atomic steps.
2. Label each step with one of the categories above.
3. Compute per-document and aggregate metrics.
4. Include representative examples of each error type in appendix material.
