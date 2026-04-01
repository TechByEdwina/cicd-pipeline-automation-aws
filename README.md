# Flask CI/CD App

A simple Flask application designed for a Jenkins + Docker + AWS EC2 CI/CD pipeline.

## What it does

Exposes two endpoints:
- `GET /` — returns a JSON confirmation that the app is running
- `GET /health` — returns a health check response

## Run locally

```bash
pip install -r requirements.txt
python app.py
```

App will be available at `http://localhost:80`

## Build and run with Docker

```bash
docker build -t flask-app .
docker run -d -p 80:80 flask-app
```

App will be available at `http://localhost`

## Pipeline overview

The Jenkinsfile defines four stages:
1. Clone — checks out the repo
2. Build — builds the Docker image
3. Push — pushes the image to DockerHub
4. Deploy — SSHs into the EC2 instance and runs the new container

Before using the pipeline, update the following in the Jenkinsfile:
- `your-dockerhub-username/app` with your actual DockerHub repo
- `your-ec2-ip` with your EC2 instance's public IP
- Add a Jenkins credential with ID `dockerhub-credentials`
