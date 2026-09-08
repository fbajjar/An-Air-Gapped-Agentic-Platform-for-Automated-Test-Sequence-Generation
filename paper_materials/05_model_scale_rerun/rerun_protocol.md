# Model-Scale Rerun Protocol

Goal: remove confound by fixing image classifier to the 4B model in all scale comparisons.

## Fixed setting
- `image_classifier_model = 4B` for every experiment variant

## Variable setting
- Vary only text generation model scale during this rerun

## Required reporting
- Table of text model scale vs. quality metrics
- Explicit note that image classifier is held constant at 4B
- Comparison to previous (confounded) results with a short interpretation
