# Plant Disease Detection System

## Setup for Python

### Install Python
Follow the instructions on the [official Python website](https://www.python.org/downloads/) to download and install Python.

### Install Python Packages
```bash
pip3 install -r training/requirements.txt
pip3 install -r api/requirements.txt
```

### Install TensorFlow Serving
Follow the instructions on the [TensorFlow Serving website](https://www.tensorflow.org/tfx/guide/serving) to set up TensorFlow Serving.

## Setup for ReactJS

### Install Node.js
Follow the instructions on the [official Node.js website](https://nodejs.org/) to download and install Node.js.

### Install NPM
NPM is installed along with Node.js. Verify the installation by running:
```bash
npm -v
```

### Install Dependencies
```bash
cd frontend
npm install --from-lock-json
npm audit fix
```

### Environment Configuration
Copy `.env.example` as `.env` and update the API URL.

## Setup for React-Native App

### React Native Environment Setup
Follow the instructions on the [React Native website](https://reactnative.dev/docs/environment-setup) under the "React Native CLI Quickstart" tab.

### Install Dependencies
```bash
cd mobile-app
yarn install
```

#### Only for Mac Users
```bash
cd ios && pod install && cd ../
```

### Environment Configuration
Copy `.env.example` as `.env` and update the API URL.

## Training the Model

### Download the Data
Download the dataset from [Kaggle](https://www.kaggle.com/). Only keep folders related to your specific plant.

### Run Jupyter Notebook
```bash
jupyter notebook
```
Open `training/plant-disease-training.ipynb` in Jupyter Notebook.

### Update Dataset Path
In cell #2, update the path to your dataset.

### Run Cells
Run all the cells one by one.

### Save the Model
Copy the generated model and save it with the version number in the `models` folder.

## Running the API

### Using FastAPI
```bash
cd api
uvicorn main:app --reload --host 0.0.0.0
```
Your API is now running at `0.0.0.0:8000`.

### Using FastAPI & TensorFlow Serve
```bash
cd api
```
Copy `models.config.example` as `models.config` and update the paths in the file.

Run TensorFlow Serving:
```bash
docker run -t --rm -p 8501:8501 -v $(pwd):/plant-disease-detection tensorflow/serving --rest_api_port=8501 --model_config_file=/plant-disease-detection/models.config
```

Run FastAPI Server:
```bash
uvicorn main-tf-serving:app --reload --host 0.0.0.0
```
Your API is now running at `0.0.0.0:8000`.

## Running the Frontend

### Navigate to Frontend Directory
```bash
cd frontend
```

### Environment Configuration
Copy `.env.example` as `.env` and update `REACT_APP_API_URL` to the API URL if needed.

### Run the Frontend
```bash
npm run start
```

## Running the Mobile App

### Navigate to Mobile App Directory
```bash
cd mobile-app
```

### Environment Configuration
Copy `.env.example` as `.env` and update the API URL if needed.

### Run the App (Android/iOS)
```bash
npm run android
```
OR
```bash
npm run ios
```

## Creating Public (Signed APK)
Follow the React Native documentation for creating a signed APK.

## Creating the TF Lite Model

### Run Jupyter Notebook
```bash
jupyter notebook
```
Open `training/tf-lite-converter.ipynb` in Jupyter Notebook.

### Update Dataset Path
In cell #2, update the path to your dataset.

### Run Cells
Run all the cells one by one.

### Save the Model
The model will be saved in the `tf-lite-models` folder.

## Deploying the TF Lite Model on GCP

### Create a GCP Account
Follow the instructions on the [Google Cloud Platform](https://cloud.google.com/) to create an account.

### Create a Project on GCP
Create a new project and note the project ID.

### Create a GCP Bucket
Upload the `plant-model.h5` model in the bucket under the path `models/plant-model.h5`.

### Install Google Cloud SDK
Follow the instructions on the [Google Cloud SDK website](https://cloud.google.com/sdk/docs/install).

### Authenticate with Google Cloud SDK
```bash
gcloud auth login
```

### Run the Deployment Script
```bash
cd gcp
gcloud functions deploy predict_lite --runtime python38 --trigger-http --memory 512 --project [PROJECT_ID]
```
Your model is now deployed.

### Test with Postman
Use Postman to test the GCF using the Trigger URL.

## Deploying the TF Model (.h5) on GCP

### Create a GCP Account
Follow the instructions on the [Google Cloud Platform](https://cloud.google.com/) to create an account.

### Create a Project on GCP
Create a new project and note the project ID.

### Create a GCP Bucket
Upload the `.h5` model in the bucket under the path `models/plant-model.h5`.

### Install Google Cloud SDK
Follow the instructions on the [Google Cloud SDK website](https://cloud.google.com/sdk/docs/install).

### Authenticate with Google Cloud SDK
```bash
gcloud auth login
```

### Run the Deployment Script
```bash
cd gcp
gcloud functions deploy predict --runtime python38 --trigger-http --memory 512 --project [PROJECT_ID]
```
Your model is now deployed.

