# End-to-End Chest Cancer Classification using MLflow & DVC

## Overview
This project implements an end-to-end pipeline for chest cancer classification using machine learning. The pipeline includes data ingestion, model training, evaluation, and deployment with AWS CI/CD and GitHub Actions. 

## Project Structure
```
cnnClassifier/
├── .github/
│   └── workflows/
│       └── .gitkeep
├── src/
│   └── cnnClassifier/
│       ├── __init__.py
│       ├── components/
│       │   ├── __init__.py
│       │   ├── data_ingestion.py
│       │   ├── prepare_base_model.py
│       │   ├── model_trainer.py
│       │   ├── model_evaluation_mlflow.py
│       ├── utils/
│       │   ├── __init__.py
│       ├── config/
│       │   ├── __init__.py
│       │   ├── configuration.py
│       ├── pipeline/
│       │   ├── __init__.py
│       │   ├── stage_01_data_ingestion.py
│       │   ├── stage_02_prepare_base_model.py
│       │   ├── stage_03_model_trainer.py
│       │   ├── stage_04_model_evaluation.py
│       │   ├── prediction.py
│       ├── entity/
│       │   ├── __init__.py
│       ├── constants/
│       │   ├── __init__.py
│       │   ├── constants.py
├── config/
│   ├── config.yaml
├── dvc.yaml
├── params.yaml
├── requirements.txt
├── setup.py
├── research/
│   ├── trials.ipynb
│   ├── dataset/
│   ├── model/
├── templates/
│   ├── index.html
├── logs/
│   ├── running_logs.log
├── main.py
```

## Features
- **Automated Data Ingestion:** Downloads dataset from Google Drive using `gdown`, extracts and stores it.
- **Model Preparation:** Uses VGG16 architecture, initializes model parameters, and prepares a base model.
- **Model Training:** Trains the model with TensorFlow and logs metrics using MLflow.
- **Model Evaluation:** Evaluates the trained model on test data and generates performance metrics.
- **Model Persistence:** Saves the trained model for future inference and deployment.
- **MLflow & DVC Integration:** Tracks experiments and manages data and model versions.
- **AWS CI/CD Deployment:** Deploys the model with Docker, AWS EC2, and ECR using GitHub Actions.

## Installation & Setup
### 1. Clone the Repository
```sh
git clone https://github.com/your-repo-name.git
cd your-repo-name
```

### 2. Create Virtual Environment & Install Dependencies
```sh
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt
```

### 3. Setup Environment Variables
```sh
export MLFLOW_TRACKING_URI=https://dagshub.com/your-mlflow-uri
export MLFLOW_TRACKING_USERNAME=your-username
export MLFLOW_TRACKING_PASSWORD=your-password
```

### 4. Initialize DVC
```sh
dvc init
dvc repro
dvc dag
```

### 5. Run the ML Pipeline
```sh
python main.py
```

## MLflow Setup
To launch the MLflow UI, run:
```sh
mlflow ui
```

## AWS Deployment Steps
### 1. Login to AWS Console & Create IAM User
- Enable `AmazonEC2ContainerRegistryFullAccess` & `AmazonEC2FullAccess`

### 2. Create ECR Repository
- Save the ECR URI: `278767805212.dkr.ecr.us-east-2.amazonaws.com/chest`

### 3. Create EC2 Instance
- Install Docker:
```sh
sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### 4. Configure EC2 as Self-hosted Runner
- Navigate to GitHub `Settings > Actions > Runner` and follow the setup instructions.

### 5. Setup GitHub Secrets
```
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=us-east-1
AWS_ECR_LOGIN_URI=278767805212.dkr.ecr.us-east-2.amazonaws.com
ECR_REPOSITORY_NAME=chest
```

### 6. Deploy to AWS
GitHub Actions will automatically build and push the Docker image to ECR and deploy it on EC2.

## Contributing
Feel free to fork the repository, make improvements, and submit pull requests.

