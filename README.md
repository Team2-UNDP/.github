# .github

Web Application Development Documentation
1. Overview
This documentation provides a guide to setting up the development environment, installing necessary tools, and deploying the application using AWS. The backend will be developed using FastAPI for real-time video communication, while the frontend is built using Next.js.
2. Tech Stack
Frontend: Next.js (React Framework)
Backend: FastAPI (for real-time video streaming)
Machine Learning: YOLOv11 (for object detection)
Database: MongoDB (NoSQL)
UI/UX Design: Figma
Data Annotation: Roboflow
Backend Deployment: AWS EC2, AWS S3 (for media storage)
Frontend Deployment: Vercel
Model Development: TensorFlow/PyTorch
Logging: JSON-based logging
Version Control & CI/CD: GitHub

3. GitHub Integration
3.1 Framework Structure
Frontend: Hosted on GitHub, deployed on Vercel
Backend: Hosted on GitHub, deployed on AWS
Model Development: Managed in a separate repository for version control
3.2 GitHub Actions (CI/CD)
Set up GitHub Actions for automated deployment in .github/workflows/deploy.yml:
name: Deploy to AWS
on: [push]


4. Development Environment Setup
4.1 Prerequisites
Ensure the following are installed:
Node.js (for Next.js frontend)
Python 3.9+
MongoDB (Local or Cloud)
Git
Docker (optional for containerization)
4.2 Setting Up the Frontend (Next.js)
# Install Node.js dependencies
git clone <repo-url>
cd frontend
npm install
npm run dev

The frontend runs on http://localhost:3000/ by default.
4.3 Setting Up the Backend (Flask + FastAPI)
Install Dependencies
git clone <repo-url>
cd backend
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt

Structure the Backend
backend/
├── app/
│   ├── flask_app.py  # Flask API (Model & Database)
│   ├── fastapi_app.py  # FastAPI (Real-time Video Communication)
│   ├── models/  # Machine Learning models
│   ├── routes/  # API routes
│   ├── static/  # Static files
│   ├── templates/  # Frontend templates
│   ├── __init__.py
│   ├── config.py  # Configuration settings
├── requirements.txt
├── run.py  # Entry point to run both APIs

Install FastAPI & Uvicorn
pip install fastapi uvicorn

CORS Configuration for Security
Modify flask_app.py to enable CORS:
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)  # Allow all domains (for development)

@app.route("/flask-endpoint")
def flask_endpoint():
    return {"message": "Hello from Flask"}

Modify fastapi_app.py to enable CORS:
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

fastapi_app = FastAPI()

fastapi_app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


5. Database Setup (MongoDB)
5.1 Local MongoDB Setup (Optional for Development)
Install MongoDB


Download MongoDB from MongoDB Community Server.
Install it following the instructions for your OS.
Start MongoDB

 mongod --dbpath /path/to/data/db


The default MongoDB server runs on mongodb://localhost:27017/.
Access Mongo Shell (Optional)

 mongo


5.2 Using MongoDB Atlas (Cloud Deployment)
Create a Free Cluster


Go to MongoDB Atlas and create an account.
Create a Shared Cluster (Free Tier).
Choose AWS as the provider (since the backend is on AWS).
Get Connection String


In MongoDB Atlas, navigate to Database > Connect > Drivers.
Copy the MongoDB URI.
Update Flask Configuration
 In config.py:

 MONGO_URI = "your-mongodb-uri"


Connect Flask to MongoDB
 Install pymongo:

 pip install pymongo



6. Frontend Deployment on Vercel
6.1 Connect Repository to Vercel
Push your Next.js frontend code to GitHub.
Go to Vercel and sign in with GitHub.
Click New Project and select the frontend repository.
Configure project settings (leave default for Next.js).
Click Deploy.
6.2 Set Up Environment Variables
In Vercel, go to Project Settings > Environment Variables.
Add required variables (e.g., API URLs, keys).
Redeploy the project for changes to take effect.
6.3 Custom Domain (Optional)
In Project Settings > Domains, add a custom domain.
Update DNS records to point to Vercel.

7. Model Development Setup
7.1 Setting Up Python Environment
# Create a virtual environment
python -m venv model_env
source model_env/bin/activate  # On Windows use `model_env\Scripts\activate`

7.2 Installing Dependencies
pip install tensorflow torch torchvision numpy pandas opencv-python

7.3 Logging Setup (JSON Logging)
Modify flask_app.py to use JSON logging:
import logging
import json

def setup_logger():
    logger = logging.getLogger("flask_app")
    handler = logging.StreamHandler()
    formatter = logging.Formatter(json.dumps({"level": "%(levelname)s", "message": "%(message)s"}))
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    logger.setLevel(logging.INFO)
    return logger

logger = setup_logger()
logger.info("Flask app started")

This setup ensures that logs are stored in a structured JSON format.


