# EMR No-Show Risk Predictor

[![Python](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/streamlit-1.15-orange)](https://streamlit.io/)
[![FastAPI](https://img.shields.io/badge/fastapi-0.70-green)](https://fastapi.tiangolo.com/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Installation](#installation)
* [Usage](#usage)

  * [Streamlit App](#streamlit-app)
  * [FastAPI Service](#fastapi-service)
* [API Example](#api-example)
* [Data](#data)
* [Model Training](#model-training)
* [Future Work](#future-work)
* [Contributing](#contributing)
* [License](#license)

---

## Overview

This repository houses an end-to-end solution for predicting patient no-show risk using historical outpatient appointment data. It includes:

* **Feature Engineering**: Lead time, temporal, demographic, and neighborhood encodings
* **Model**: A supervised classifier saved as `no_show_model.pkl`
* **Service**: FastAPI-based inference endpoint in `emrapp.py`
* **UI**: Interactive Streamlit application in `streamlit_app.py`

By identifying high-risk appointments, healthcare providers can implement targeted reminders and reduce operational waste.

## Features

* **Interactive Dashboard**: Enter appointment details and view no-show probability
* **REST API**: Lightweight inference endpoint for integration
* **Modular Pipeline**: Clear separation of preprocessing, encoding, and prediction
* **Extensible**: Easily add new features or data sources

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/EMR_NoShow_Risk_Predictor.git
   cd EMR_NoShow_Risk_Predictor
   ```
2. Create and activate a virtual environment:

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # on Windows: venv\Scripts\activate
   ```
3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

## Usage

### Streamlit App

Launch the interactive UI for manual testing:

```bash
streamlit run streamlit_app.py
```

### FastAPI Service

Start the FastAPI server for programmatic access:

```bash
uvicorn emrapp:app --reload
```

The service will run at `http://127.0.0.1:8000`.

## API Example

Send a POST request to `/predict_no_show` with JSON payload:

```json
POST /predict_no_show HTTP/1.1
Host: 127.0.0.1:8000
Content-Type: application/json

{
  "patient_id": "29872499824296",
  "appointment_date": "2016-04-29",
  "scheduled_date": "2016-04-29",
  "appointment_time": "22:16",
  "scheduled_time": "22:16",
  "age": 62,
  "gender": "F",
  "neighbourhood": "JARDIM DA PENHA",
  "scholarship": false,
  "hypertension": true,
  "diabetes": false,
  "alcoholism": false,
  "handicap": false,
  "sms_received": true
}
```

Response:

```json
{
  "no_show_probability": 0.9895
}
```

## Data

The model is trained on historical appointment records containing demographic and clinical flags. Raw tables:

* `appointments_raw`
* `patients_raw`
* `conditions_raw`

## Model Training

Feature engineering and training scripts (not included) follow these steps:

1. Clean and normalize timestamps
2. Compute lead time and temporal attributes
3. Encode categorical variables and neighbourhood via target encoding
4. Train classifier with cross-validation
5. Serialize artifacts: `neighborhood_encoder.pkl`, `no_show_model.pkl`

## Future Work

* **Power BI Dashboard**: Integrate prediction outputs and clinic analytics (*coming soon*)
* **Additional Features**: Prior attendance history, socio-economic factors
* **Evaluation**: Report precision, recall, ROC–AUC metrics

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with proposed changes.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
