
# Spring Boot CI/CD Pipeline with Jenkins, Docker & Kubernetes (Minikube)

This project demonstrates a fully automated CI/CD pipeline for deploying a Spring Boot application using **Jenkins**, **Docker**, and **Kubernetes (Minikube)**.  
The pipeline is triggered by GitHub updates and performs end-to-end automation: building the application, containerizing it, pushing the image to DockerHub, and deploying it to a Kubernetes cluster.

---

## Pipeline Workflow

The Jenkins pipeline executes the following stages automatically when a push or pull-request merge occurs on GitHub:

1. **Clone Source Code**  
   Jenkins pulls the latest version of the Spring Boot project from GitHub.

2. **Build & Package Application**  
   Maven/Gradle builds the project and creates a fresh JAR file.

3. **Containerize Application**  
   Jenkins builds a Docker image using the `Dockerfile`.

4. **DockerHub Authentication**  
   Secure login to DockerHub using Jenkins credentials (stored as secrets).

5. **Push Docker Image to DockerHub**  
   The newly built image is tagged and pushed to a DockerHub repository.

6. **Kubernetes Deployment**  
   Jenkins applies the `deployment.yaml` to deploy the application on the Minikube cluster.

7. **Kubernetes Service Creation**  
   Jenkins applies the `service.yaml` to expose the application via a NodePort service.

---

## Architecture

```text
GitHub → Jenkins Pipeline → Docker Build → DockerHub Push → Kubernetes (Minikube)
````

### Tools Used

| Layer          | Technology                    |
| -------------- | ----------------------------- |
| CI/CD Engine   | Jenkins                       |
| Containers     | Docker                        |
| Orchestration  | Kubernetes (Minikube)         |
| Source Control | GitHub                        |
| Registry       | DockerHub                     |
| Application    | Spring Boot (single endpoint) |

---

## Kubernetes Deployment Overview

* Deployed as a **Deployment** resource.
* Initially runs with **1 Pod** and can be scaled up to **2+ Pods**.
* Exposed using a **NodePort** service for local access via Minikube.

Start Minikube:

```bash
minikube start
```

Open Kubernetes dashboard (optional):

```bash
minikube dashboard
```

Scale deployment:

```bash
kubectl scale deployment myapp-deployment --replicas=2
```

---

## Running the Application

### 1. Start Minikube

```bash
minikube start
```

### 2. Build & Push Docker Image (via Jenkins)

The Jenkins pipeline will:

* Build the JAR
* Build the Docker image
* Push it to DockerHub
* Apply the Kubernetes manifests

Alternatively, you can apply the manifests manually:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 3. Access the Application

Get the service URL:

```bash
minikube service myapp-service
```

This will open the exposed endpoint in your browser or print the URL.

---

## Features

* End-to-end **CI/CD pipeline** with Jenkins
* **Automated build → Dockerize → Push → Deploy** flow
* **GitHub-triggered** pipeline (push / merge)
* **DockerHub integration** with secure credentials
* **Kubernetes deployment** and service on Minikube
* **Scalable** microservice (multiple pods)

## Outcome

This project showcases a modern DevOps workflow for Java/Spring applications, combining continuous integration and continuous delivery with containerization and Kubernetes orchestration.
It can serve as a template or starting point for more advanced production-grade CI/CD setups.


