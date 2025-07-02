# Vehicle Insurance Forecasting

## Objective

The Vehicle Insurance Forecasting project aims to build a robust machine learning pipeline that predicts vehicle insurance outcomes based on historical and customer data. The solution is designed with modular components, model monitoring, cloud storage integration (MongoDB and AWS S3), and a full CI/CD pipeline for automated deployment and retraining.

---

## Workflow Architecture

The project follows a modular MLOps workflow with the following main stages:

1. **Data Ingestion**: Fetches data from MongoDB, splits into train/test, and stores locally.
2. **Data Validation**: Validates schema and performs data quality checks.
3. **Data Transformation**: Cleans, transforms, and encodes data; applies SMOTEENN for imbalanced classes.
4. **Model Training**: Trains ML models using the transformed data and evaluates accuracy.
5. **Model Evaluation**: Compares new model performance against previously deployed models.
6. **Model Pusher**: Pushes accepted models to AWS S3 as a model registry.
7. **Prediction Pipeline**: Provides an API endpoint for real-time predictions.
8. **CI/CD Pipeline**: Automated with Docker, GitHub Actions, and AWS resources for continuous integration and deployment.

---

## Pipeline Diagram

```
+-------------------+
|   Data Ingestion  | <-- MongoDB
+---------+---------+
          |
          v
+---------+---------+
|  Data Validation  |
+---------+---------+
          |
          v
+---------+---------+
| Data Transformation |
+---------+---------+
          |
          v
+---------+---------+
|  Model Trainer    |
+---------+---------+
          |
          v
+---------+---------+
| Model Evaluation  |<--->| Model Registry (S3) |
+---------+---------+           ^
          |                     |
          v                     |
+---------+---------+           |
|  Model Pusher     +-----------+
+-------------------+
          |
          v
+-------------------+
| Prediction API    |
+-------------------+
```

---

## Model Architecture

- **Data Preprocessing**: Pipelines built with scikit-learn for feature engineering and transformation.
- **Handling Imbalance**: SMOTEENN is used to address class imbalance.
- **Model**: The pipeline supports various classifiers; configurations for RandomForest-like models with hyperparameters such as `n_estimators`, `min_samples_split`, `max_depth`, `criterion`, etc., are specified in the project constants.
- **Evaluation**: Models are evaluated using F1-score, precision, and recall. Only models exceeding the previous best score are pushed to production.

---

## How to Setup and Run

### 1. Clone & Environment Setup

```bash
git clone https://github.com/Sabarna07-tech/Vehicle_Insurance_Forecasting.git
cd Vehicle_Insurance_Forecasting
conda create -n vehicle python=3.10 -y
conda activate vehicle
pip install -r requirements.txt
```

### 2. MongoDB Setup

- Sign up at MongoDB Atlas, create a cluster, user, and set network access to `0.0.0.0/0`.
- Get the Python (3.6+) connection string and set it as an environment variable:

```bash
export MONGODB_URL="mongodb+srv://<username>:<password>@cluster-url"
```

- Add your dataset to the `notebook` folder and push it to MongoDB using a Jupyter notebook.

### 3. AWS Setup

- Create an S3 bucket (e.g., `my-model-mlopsproj`) in `us-east-1`.
- Set up IAM user, access keys, and add them as environment variables.
- Update constants in `src/constants/__init__.py` with your bucket and key names.

### 4. Running the Pipeline Locally

```bash
python demo.py
```
- This will run the complete pipeline: ingestion, validation, transformation, training, evaluation, and pushing the model to S3.

### 5. Launch the API

```bash
python app.py
```
- Visit `http://localhost:5000` (or your deployed endpoint) to use the prediction API.

### 6. CI/CD and Deployment

- Build the Docker image:
  ```bash
  docker build -t vehicle-insurance-app .
  ```
- Push to AWS ECR and deploy via EC2 with Docker, following the guide in `projectflow.txt`.
- Set up GitHub Actions for CI/CD using workflow files in `.github/workflows/`.

---

## Additional Resources

- Full project flow and commands: [projectflow.txt](https://github.com/Sabarna07-tech/Vehicle_Insurance_Forecasting/blob/main/projectflow.txt)
- Model, pipeline, and utility code: See the `src/` directory for modular implementations.
- For extending or customizing components, refer to the config files in the `config/` directory.

---

## Note

This summary is based on the current repository structure and may not reflect the latest changes. For more details or updates, please refer directly to the [repository contents](https://github.com/Sabarna07-tech/Vehicle_Insurance_Forecasting/tree/main).
