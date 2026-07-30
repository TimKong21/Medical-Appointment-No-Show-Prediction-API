# Medical Appointment No-Show Prediction API

An end-to-end machine-learning API for scoring the likelihood that a patient will miss a scheduled medical appointment. It turns appointment data into no-show predictions so healthcare operations teams can prioritize outreach and improve schedule utilization.

<p align="center">
  <img src="assets/medical-appointment-no-show-api-hero.png" alt="Illustration of a medical appointment prediction API" width="85%">
</p>

## Product overview

The project trains an XGBoost classifier on historical appointment records, packages the model for Amazon SageMaker, and exposes predictions through AWS Lambda and Amazon API Gateway. Prediction results are saved to Amazon S3 for downstream use.

> This is a portfolio and educational project. Its predictions should support—not replace—clinical or operational judgment, and the included dataset must not be used with real patient data without the appropriate privacy, security, and governance controls.

## What it does

- Scores appointment records for no-show risk with a trained XGBoost model.
- Accepts appointment data as `text/csv` and applies the same preprocessing used for training.
- Engineers time-to-appointment, date, age-band, gender, and neighbourhood features.
- Runs the model in Amazon SageMaker behind AWS Lambda and Amazon API Gateway.
- Writes prediction outputs to Amazon S3.

## Architecture

```text
Appointment CSV
      │
      ▼
API Gateway → AWS Lambda → Amazon SageMaker endpoint
                                  │
                                  ▼
                         XGBoost inference pipeline
                                  │
                                  ▼
                       Predictions returned + stored in S3
```

## Input and output

The SageMaker inference handler accepts a CSV payload with these appointment attributes:

```text
PATIENTID, APPOINTMENTID, GENDER, SCHEDULEDDAY, APPOINTMENTDAY, AGE,
NEIGHBOURHOOD, SCHOLARSHIP, HIPERTENSION, DIABETES, ALCOHOLISM,
HANDCAP, SMS_RECEIVED
```

The model returns no-show predictions and adds a `PREDICTED_NO_SHOW` field to the prediction file saved in S3. See [`data/input/simulated_set.csv`](data/input/simulated_set.csv) for an example payload shape.

## Tech stack

| Area | Tools |
| --- | --- |
| Modeling | Python, pandas, scikit-learn, XGBoost |
| Data platform | Snowflake, SQL |
| Deployment | Amazon S3, Amazon SageMaker, AWS Lambda, Amazon API Gateway |
| Optimization | Hyperopt, SMOTE |

## Repository guide

```text
.
├── src/                  # Modular data prep, feature engineering, training, and local prediction
├── deployment_assets/    # SageMaker inference code and model deployment artifacts
├── data/                 # Input, processed, feature, model-output, and tuning artifacts
├── Snowflake_assets/     # SQL scripts and source data used for analysis
├── model/                # Trained XGBoost model
├── assets/               # README visual assets
├── Project Notebook.ipynb
├── Model Deployment.ipynb
└── Project Documentation.pdf
```

## Run locally

```bash
git clone https://github.com/TimKong21/Medical-Appointment-No-Show-Prediction-API.git
cd Medical-Appointment-No-Show-Prediction-API

python -m venv .venv
```

Activate the environment, then install dependencies:

```bash
# Windows
.venv\Scripts\activate

# macOS/Linux
source .venv/bin/activate

pip install -r src/requirements.txt
```

From `src/`, train and run local prediction:

```bash
python train.py
python predict.py
```

## Deployment and project materials

- [`Model Deployment.ipynb`](Model%20Deployment.ipynb) documents the SageMaker, Lambda, and API Gateway deployment flow.
- [`Project Notebook.ipynb`](Project%20Notebook.ipynb) contains the exploratory analysis and model development work.
- [`Project Documentation.pdf`](Project%20Documentation.pdf) provides the original project documentation.
- [Video walkthrough](https://youtu.be/EyFcrpPCROk?si=URmuMBRbyzrwR39R)

## Dataset

This project uses the [Kaggle Medical Appointment No Shows dataset](https://www.kaggle.com/datasets/joniarroba/noshowappointments). Please review the dataset license and terms before reuse.


## License

The source code in this repository is available under the [MIT License](LICENSE). The included [Kaggle dataset](https://www.kaggle.com/datasets/joniarroba/noshowappointments) remains subject to its own license and terms of use.
