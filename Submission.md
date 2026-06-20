# Problem Statement: Multi-Modal Evidence Review for Damage Claims

## Overview

Build a system that verifies damage claims using images, a short claim conversation, user history, and minimum evidence requirements.

Each claim is about one of three object types:
- **car**
- **laptop**
- **package**

## Task

For each claim, the system should:

1. Extract the actual damage claim from the conversation
2. Inspect one or more submitted images
3. Decide whether the image evidence is sufficient
4. Identify the visible issue type
5. Identify the relevant object part
6. Decide whether the claim is **supported**, **contradicted**, or **lacks enough information**
7. Select the image IDs that support the decision
8. Flag image quality, mismatch, authenticity, or user-history risks
9. Estimate severity
10. Produce short justifications grounded in the images

## Input Schema

Claims CSV contains:
- `claim_id`: Unique identifier
- `user_id`: User identifier
- `conversation`: Text conversation about the claim
- `image_ids`: Semicolon-separated image IDs
- `object_type`: car / laptop / package
- `expected_*` columns for validation (optional)

## Output Schema

The output CSV must contain:

| Column | Type | Description |
|--------|------|-------------|
| claim_id | string | From input |
| decision | string | supported / contradicted / insufficient_evidence |
| confidence | float | 0.0 to 1.0 |
| supporting_image_ids | string | Semicolon-separated |
| damage_type | string | Identified damage type |
| object_part | string | Affected part |
| severity | string | minor / moderate / severe / total_loss |
| risk_flags | string | Semicolon-separated |
| justification | string | Grounded explanation |
| extracted_claim | string | Claim summary from conversation |

## Evaluation

Metrics evaluated:
- Decision accuracy
- Severity accuracy
- Damage type accuracy
- Object part accuracy
- Precision/Recall per decision class
- Overall F1 score

## Requirements

1. Must read provided CSV files and local images
2. Must produce output.csv with exact schema
3. Must include evaluation workflow
4. Must avoid hardcoded test labels or file-specific answers

## Submission

1. **Code zip**: Code folder (exclude venvs, node_modules, build artifacts, data folder)
2. **Predictions CSV**: output.csv for test data
3. **Chat transcript**: log.txt
