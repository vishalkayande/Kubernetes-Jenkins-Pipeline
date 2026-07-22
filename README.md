# 🚀 Automated CI/CD Pipeline for K8s Web Deployment Using Jenkins !

A fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline engineered to streamline web application rollouts. I built this architecture to achieve zero-downtime deployments, fully automating the journey from a simple GitHub code push directly to a live Kubernetes cluster.

## ⚙️ Tech Stack

*   **Source Control:** Git & GitHub
*   **CI/CD Engine:** Jenkins
*   **Containerization:** Docker & Docker Hub
*   **Orchestration:** Kubernetes (K8s)
*   **Web Server:** Nginx
*   **Infrastructure:** Linux Virtual Machines (Ubuntu)

## 🏗️ System Architecture

This project spans across three distinct environments: a source code repository, a dedicated Jenkins CI server, and a two-node Kubernetes orchestration cluster.

1.  **Code Commit:** Code updates (HTML/CSS) are pushed to the `main` branch on GitHub.
2.  **Continuous Integration:** A GitHub Webhook detects the push and instantly triggers the Jenkins pipeline.
3.  **Container Build:** Jenkins clones the repository, builds a new Docker image, and dynamically tags it with the unique Jenkins `$BUILD_NUMBER`.
4.  **Registry Push:** Jenkins securely authenticates with Docker Hub and pushes the versioned container image.
5.  **Continuous Deployment:** Jenkins connects to the Kubernetes Master Node, dynamically updates the `deployment.yaml` with the new image tag, and applies the rollout.
6.  **User Access:** The updated web application is instantly accessible to end-users via a Kubernetes NodePort.

## 📂 Repository Structure

```text
├── Dockerfile              # Instructions to containerize the Nginx web application
├── Jenkinsfile             # Declarative multi-stage Jenkins pipeline script
├── index.html              # The main web application source code
└── k8s/
    ├── deployment.yaml     # Defines the K8s Pod replicas and container specs
    └── service.yaml        # Exposes the application to the internet via NodePort
