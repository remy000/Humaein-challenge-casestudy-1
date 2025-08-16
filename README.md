# Case Study 1: Claim Resubmission Ingestion Pipeline

## Overview

This healthcare claims processing pipeline processes insurance claim data from multiple Electronic Medical Records (EMR) systems, normalizes different schemas, and determines resubmission eligibility using intelligent business rules and mock AI classification.

## Key Features

- **Data Ingestion**: Robust processing of multiple EMR sources with different formats (CSV and JSON)
- **Schema Normalization**: Advanced data wrangling and mapping skills to handle inconsistent data
- **Eligibility Analysis**: Clear, testable logic for resubmission eligibility determination
- **Mock LLM Classifier**: Intelligent classification for ambiguous denial reasons
- **Comprehensive Error Handling**: Graceful processing of malformed, missing, and inconsistent data
- **Modular Architecture**: Object-oriented design with specialized processing components
- **Output Generation**: JSON output for downstream automation systems
- **Comprehensive Logging**: Detailed logging for debugging and audit trails
- **Metrics Collection**: Performance and business metrics tracking

## Project Structure

```
ch1/
├── healthcare_pipeline_notebook.ipynb  # Main Jupyter notebook implementation
├── emr_alpha.csv                      # Sample EMR Alpha data (CSV format)
├── emr_beta.json                      # Sample EMR Beta data (JSON format)
├── requirements.txt                   # Python dependencies
├── README.md                          # This documentation
├── .gitignore                         # Git ignore patterns
├── healthcare_pipeline.log            # Execution logs (generated)
├── resubmission_candidates.json       # Output: eligible claims (generated)
└── pipeline_metrics.json              # Output: execution metrics (generated)
```

## Usage

### Run the Jupyter Notebook

1. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

2. **Start Jupyter**:
   ```bash
   jupyter notebook
   ```

3. **Open and run the notebook**:
   - Open `healthcare_pipeline_notebook.ipynb`
   - Run all cells sequentially to execute the complete pipeline
   - Review the generated outputs and analysis

## Input Data Formats

### EMR Alpha (CSV Format)
```csv
claim_id,patient_id,procedure_code,denial_reason,submitted_at,status
A123,P001,99213,Missing modifier,2025-07-01,denied
```

### EMR Beta (JSON Format)
```json
[
  {
    "id": "B987",
    "member": "P010",
    "code": "99213",
    "error_msg": "Incorrect provider type",
    "date": "2025-07-03T00:00:00",
    "status": "denied"
  }
]
```

## Business Rules

### Resubmission Eligibility Criteria

A claim is flagged for resubmission if **ALL** conditions are met:

1. **Status is 'denied'**
2. **Patient ID is not null**
3. **Submitted more than 7 days ago** (from reference date: 2025-07-30)
4. **Denial reason is retryable**

### Denial Reason Classification

- **Retryable**: "Missing modifier", "Incorrect NPI", "Prior auth required"
- **Non-Retryable**: "Authorization expired", "Incorrect provider type"
- **Ambiguous**: Classified using intelligent heuristics (e.g., "incorrect procedure" → retryable)

## Output Format

### Resubmission Candidates (`resubmission_candidates.json`)
```json
[
  {
    "claim_id": "A124",
    "resubmission_reason": "Incorrect NPI",
    "source_system": "alpha",
    "recommended_changes": "Review NPI number and resubmit"
  }
]
```

### Pipeline Metrics (`pipeline_metrics.json`)
```json
{
  "execution_timestamp": "2025-08-14T10:30:00",
  "total_claims_processed": 9,
  "claims_by_source": {"alpha": 5, "beta": 4},
  "flagged_for_resubmission": 3,
  "excluded_claims": 6,
  "exclusion_reasons": {...},
  "success_rate": 100.0
}
```

## Configuration

Key configuration parameters in the pipeline:

- **Reference Date**: `2025-07-30` (configurable)
- **Minimum Days for Resubmission**: `7 days` (configurable)
- **Classification Rules**: Easily extensible denial reason mappings

## Error Handling

The pipeline includes comprehensive error handling for:

- **File Issues**: Missing files, empty files, malformed data
- **Processing Errors**: Invalid dates, malformed records, schema mismatches
- **Output Issues**: File save failures, validation errors
- **Logging**: All errors logged with context for debugging

## Metrics and Reporting

The pipeline tracks and reports:

- **Processing Summary**: Total claims, success rates, error counts
- **Source Breakdown**: Claims distribution by EMR system
- **Eligibility Analysis**: Resubmission rates, exclusion reasons
- **Business Impact**: Revenue recovery opportunities
- **Quality Metrics**: Data quality indicators

## Testing

The pipeline can be tested with:

1. **Provided Sample Data**: EMR Alpha (CSV) and Beta (JSON) files included
2. **Edge Cases**: The notebook handles empty files, malformed data, and missing fields
3. **Business Logic Validation**: Verify eligibility rules with the included test cases
4. **Output Validation**: Ensures all required fields are present and correctly formatted

## Expected Outputs

After running the notebook, you'll find:

- **`resubmission_candidates.json`**: Claims eligible for resubmission
- **`pipeline_metrics.json`**: Detailed execution metrics and statistics
- **`healthcare_pipeline.log`**: Complete execution log for debugging

## Support

For questions or issues:

1. Check the execution logs in the notebook output cells
2. Review `healthcare_pipeline.log` for detailed error information
3. Examine `pipeline_metrics.json` for execution statistics
4. The notebook includes step-by-step analysis and explanations
