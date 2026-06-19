# Damage Claims Verification System

A multi-modal evidence review system that verifies damage claims using images, conversation context, and user history.

## Project Structure

```
project/
├── agent/                    # Python verification agent
│   ├── core/                  # Core modules (config, models, prompts)
│   ├── services/              # VLM service for image analysis
│   └── evaluation/            # Evaluation metrics
├── data/                      # Sample and test data
│   ├── train/                 # Training data
│   ├── test/                  # Test data
│   └── images/                # Image repository
├── output/                    # Outputs (CSV, reports, logs)
├── src/                       # React frontend
│   ├── components/            # UI components
│   ├── pages/                 # Page views
│   └── lib/                   # Utilities and API
├── run_agent.py               # CLI entry point
├── test_agent.py              # Test suite
└── requirements.txt           # Python dependencies
```

## Setup

### Python Backend

```bash
# Install Python dependencies
pip install -r requirements.txt

# Set API keys (optional - for VLM features)
export OPENAI_API_KEY=your_key
# or
export ANTHROPIC_API_KEY=your_key
```

### Frontend

```bash
# Install npm dependencies
npm install

# Run development server (runs automatically in Bolt)
npm run dev
```

## Usage

### Running the Agent

```bash
# Verify claims from CSV
python run_agent.py run --input data/sample_claims.csv --output output/results.csv

# Evaluate results against ground truth
python run_agent.py evaluate --predictions output/results.csv --ground-truth data/sample_claims.csv

# Run complete pipeline
python run_agent.py complete --test-input data/test_claims.csv --output-dir output
```

### Testing

```bash
# Run unit tests
python test_agent.py
```

## Output Schema

The system produces `output.csv` with the following columns:

| Column | Description |
|--------|-------------|
| claim_id | Unique claim identifier |
| decision | supported / contradicted / insufficient_evidence |
| confidence | Confidence score (0-1) |
| supporting_image_ids | Image IDs supporting decision (; separated) |
| damage_type | Detected damage type |
| object_part | Affected object part |
| severity | minor / moderate / severe / total_loss |
| risk_flags | Risk indicators (; separated) |
| justification | Decision explanation |
| extracted_claim | Extracted claim summary |

## Submission Files

For HackerRank submission:

1. **Code Zip**: Exclude `node_modules/`, `data/`, `__pycache__/`, virtual envs
2. **Predictions CSV**: `output/output.csv`
3. **Chat Transcript**: `output/log.txt`

## Architecture

### Verification Pipeline

1. **Claim Extraction**: Parses the conversation to identify claimed object, damage type, and severity
2. **Image Analysis**: Uses VLM (or fallback rules) to analyze submitted images
3. **Decision Synthesis**: Combines evidence to make final verification decision

### Decision Types

- **Supported**: Images clearly show the claimed damage
- **Contradicted**: Images show no such damage or different damage
- **Insufficient Evidence**: Images don't provide clear evidence

### Risk Flags

- `image_quality`: Poor image quality prevents analysis
- `mismatch`: Claimed damage doesn't match visual evidence
- `authenticity`: Images appear manipulated or staged
- `user_history`: User has suspicious claim history

## API Keys

The agent works without API keys using rule-based analysis. For enhanced accuracy:

- Set `OPENAI_API_KEY` for GPT-4 Vision
- Set `ANTHROPIC_API_KEY` for Claude Vision

## License

MIT
