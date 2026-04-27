# Docker CI/CD Pipeline using Jenkins + GitHub Actions

## Project Overview

This project demonstrates an automated CI/CD pipeline using **Jenkins**, **GitHub Actions**, **Docker**, **GitHub**, and **Docker Hub**. Whenever source code is updated in the GitHub repository, the pipeline automatically builds a Docker image, authenticates with Docker Hub, and pushes the latest image.

This project shows two modern automation methods:

- **Jenkins Pipeline**
- **GitHub Actions Workflow**


## Objective

To automate the software build and container image delivery process using DevOps tools.

## Tools & Technologies Used

- Jenkins (Pipeline Automation)
- GitHub Actions (CI/CD Workflow)
- Docker (Containerization)
- Git & GitHub (Source Control)
- Docker Hub (Image Registry)
- VS Code (Development Environment)
- WSL / Linux Terminal
- Python Flask (Sample App)

## Project Architecture

1. Developer pushes code to GitHub.
2. Jenkins or GitHub Actions gets triggered.
3. Workflow reads configuration file.
4. Docker image is built.
5. Docker Hub login is performed securely.
6. Docker image is pushed automatically.


## Repository Structure

```text
## Repository Structure

```text
repo-lab-11/
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── main.py
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
└── README.md

## Source Files

## app.py

Python Flask sample application.

```app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Docker CI/CD Running Successfully!"

app.run(host='0.0.0.0', port=5000)
'''

### requirements.txt

```txt
flask
```

## Dockerfile

```dockerfile
FROM python:3.10
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

## Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "rishukumar/repo-lab-11"
        TAG = "latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }
    }
}
```
## GitHub Actions Workflow

File Location

```.github/workflows/ci-cd.yml```

## CI-CD.yml 

```CI-CD
name: Docker CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Source Code
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_TOKEN }}

      - name: Build Docker Image
        run: docker build -t rishukumar/repo-lab-11:latest .

      - name: Push Docker Image
        run: docker push rishukumar/repo-lab-11:latest
```

### Jenkins Configuration Steps

1. Install Jenkins in Docker container.
2. Install required plugins:

   * Pipeline
     n   - Git
   * Docker Pipeline
3. Add Docker Hub credentials in Jenkins:

   * Kind: Username with password
   * ID: `dockerhub-creds`
4. Create new Pipeline Job.
5. Choose **Pipeline script from SCM**.
6. Select Git repository URL.
7. Branch: `*/main`
8. Script path: `Jenkinsfile`
9. Click **Build Now**.

## Commands Used

```bash
git add .
git commit -m "Added Jenkins + GitHub Actions pipeline"
git push origin main
```

## Successful Output

```text
Finished: SUCCESS
Workflow completed successfully
Docker image pushed successfully
```

Docker image pushed successfully to Docker Hub:

```text
rishukumar/repo-lab-11:latest
```





## Learning Outcomes

* Understanding CI/CD pipeline workflow
* Docker image creation and registry push
* Jenkins credentials management
* SCM integration with GitHub
* GitHub Actions workflow automation
* Real-world DevOps automation process

## Viva Questions & Answers

### Q1. What is Jenkins?

Jenkins is an open-source automation server used for CI/CD pipelines.

### Q32. What is GitHub Actions?

### Q3. Why Docker is used?

Docker packages applications with dependencies into portable containers.

### Q4. What is CI/CD?

Continuous Integration and Continuous Delivery automate build, test, and deployment processes.

### Q5. Why use credentials in Jenkins?

To securely store usernames, passwords, and tokens.

## Output Summary

```text
Build Status      : SUCCESS
Docker Image Name : rishukumar/docker-jenkins-project:latest
Repository        : GitHub + Docker Hub
Automation Tool   : Jenkins + GitHub Actions
```

## Conclusion

This project successfully automated the Docker image build and push process using Jenkins Pipeline. It demonstrates practical DevOps implementation used in real software delivery environments. The project integrates source control, containerization, automation, and cloud image registry into one complete workflow.
