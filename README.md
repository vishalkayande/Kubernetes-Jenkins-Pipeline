# 🚀 Automated CI/CD Pipeline for K8s Web Deployment

A fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline engineered to streamline web application rollouts. This architecture achieves zero-downtime deployments, automating the journey from a GitHub code push directly to a live Kubernetes cluster.

## ⚙️ Tech Stack
*   **Source Control:** Git & GitHub
*   **CI/CD Engine:** Jenkins
*   **Containerization:** Docker & Docker Hub
*   **Orchestration:** Kubernetes (K8s)
*   **Web Server:** Nginx
*   **Infrastructure:** Linux Virtual Machines (Ubuntu)

## 🏗️ System Architecture

```mermaid
graph LR
    classDef public bg:#f3f4f6,stroke:#9ca3af,stroke-width:2px,stroke-dasharray: 5 5;
    classDef cicd bg:#fef08a,stroke:#eab308,stroke-width:2px;
    classDef k8s bg:#bfdbfe,stroke:#3b82f6,stroke-width:2px;
    classDef worker bg:#fecaca,stroke:#ef4444,stroke-width:2px;

    User((End Users))

    subgraph "Public Internet Layer"
        GH[GitHub Repository]
        DH[(Docker Hub)]
    end
    class "Public Internet Layer" public

    subgraph "Private Virtual Machine Network"
        subgraph "Automation Tier"
            Ngrok[Ngrok Tunnel]
            Jenkins[Jenkins Server]
            Ngrok -->|Forwards| Jenkins
        end
        class "Automation Tier" cicd

        subgraph "Orchestration Tier"
            KubeAPI[Kubernetes Master Node]
        end
        class "Orchestration Tier" k8s

        subgraph "Application Tier"
            Svc[NodePort Service :30080]
            Pod[Web App Pods]
            Svc --- Pod
        end
        class "Application Tier" worker
    end

    GH -- "1. Webhook" --> Ngrok
    Jenkins -- "2. clone" --> GH
    Jenkins -- "3. docker push" --> DH
    Jenkins -- "4. kubectl apply" --> KubeAPI
    KubeAPI -- "5. Deploy" --> Pod
    Pod -. "6. Image Pull" .-> DH
    User -- "7. HTTP Access" --> Svc
