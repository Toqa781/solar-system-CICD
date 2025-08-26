# Solar System CI/CD Project

This repository contains the **Solar System web application** with a complete **CI/CD pipeline** using GitHub Actions, Docker, and Kubernetes (EKS).  

The project is based on the [Sidd-Harth Solar System app](https://github.com/sidd-harth/solar-system), with enhancements to automate testing, containerization, and deployment to AWS EKS.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Workflow](#workflow)
- [CI/CD Pipeline](#cicd-pipeline)
- [Environment Variables](#environment-variables)
- [Running Locally](#running-locally)
- [Deployment](#deployment)

---

## Project Overview

The Solar System app allows users to enter a number and display information about planets.  

Enhancements made in this project include:

- **Unit Testing & Code Coverage** using Mocha/Chai.
- **Dockerized backend** for easy deployment.
- **Kubernetes deployment** using AWS EKS.
- **CI/CD pipeline** that automates testing, containerization, and deployment.

---

## Architecture

The project follows a microservices-like architecture:

Frontend (HTML/JS)
|
v
Backend (Node.js + Express + MongoDB)
|
v
Database (MongoDB Atlas)



- **Frontend** communicates with the backend via REST API.
- **Backend** connects to MongoDB Atlas to fetch planet data.
- **Kubernetes** is used to deploy backend services on AWS EKS.
- **Docker** images are built and pushed to Docker Hub.

---

## Workflow

The workflow consists of four main stages:

1. **Unit Testing**
   - Run `npm test` for all backend tests.
   - Archive test results as artifacts.

2. **Code Coverage**
   - Run `npm run coverage`.
   - Archive coverage reports as artifacts.

3. **Containerization**
   - Build Docker image of the app.
   - Test the Docker container locally within GitHub Actions.
   - Push image to Docker Hub.

4. **Deployment**
   - Configure AWS credentials.
   - Update kubeconfig for EKS cluster.
   - Apply Kubernetes deployment and service manifests.
   - Verify pods and services.

---

## CI/CD Pipeline

The pipeline is implemented using **GitHub Actions**:

- **Trigger**: On push to `main` branch.
- **Jobs**:
  1. **Unit Testing** – Run tests and save results.
  2. **Code Coverage** – Run coverage and save reports.
  3. **Containerization** – Build, test, and push Docker images.
  4. **Deployment** – Deploy to AWS EKS using kubectl.

---

## Environment Variables

The project requires the following environment variables:

| Name             | Source              | Description                                    |
|-----------------|-------------------|-----------------------------------------------|
| MONGO_URI        | GitHub Secret      | MongoDB connection URI                        |
| MONGO_USERNAME   | GitHub Secret/Var  | MongoDB username                              |
| MONGO_PASSWORD   | GitHub Secret      | MongoDB password                              |
| DOCKERHUB_USERNAME | GitHub Var       | Docker Hub username                            |
| DOCKERHUB_TOKEN  | GitHub Secret      | Docker Hub access token                        |
| AWS_ACCESS_KEY_ID | GitHub Secret     | AWS IAM access key                             |
| AWS_SECRET_ACCESS_KEY | GitHub Secret | AWS IAM secret key                             |
| AWS_REGION       | GitHub Var         | AWS region for EKS                             |
| CLUSTER_NAME     | GitHub Var         | Name of the EKS cluster                        |

> Note: MongoDB credentials must match a user created in your MongoDB Atlas cluster.

---

## Running Locally

1. Clone the repository:

```bash
git clone https://github.com/<your-username>/solar-system-CICD.git
cd solar-system-CICD
```

2- Install dependencies:

```bash
npm install
```

3-Set environment variables:

```bash
export MONGO_URI="mongodb+srv://<username>:<password>@cluster.mongodb.net/superData"
export MONGO_USERNAME="<username>"
export MONGO_PASSWORD="<password>"
```

4-Start the application:
```bash
npm start
```

5-Open the frontend in your browser and interact with the app.


## Deployment

The project deploys automatically via GitHub Actions to AWS EKS:

Backend pods run in private subnets.

Kubernetes Service of type LoadBalancer exposes the backend.

Frontend communicates with backend via the LoadBalancer IP.

To manually deploy:

```bash
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
```


