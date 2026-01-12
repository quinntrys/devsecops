# **🛡️ DevSecOps Automation Pipeline**

## **📖 Project Overview**

This repository demonstrates a comprehensive **DevSecOps implementation** assigned as part of the advanced cloud computing curriculum. The project automates the lifecycle of a secure Node.js application—from code commit to production deployment—integrating **Continuous Integration (CI)**, **Continuous Security**, and **Continuous Deployment (CD)**.

The pipeline is executed on a **Self-Hosted Runner**, ensuring full control over the build environment, and deploys to a local **Kubernetes (Minikube)** cluster.

## **🏗️ Architecture & Workflow**

The pipeline follows a strict **"Shift-Left" Security** approach, ensuring vulnerabilities are caught before deployment.

graph LR  
    A\[💻 Push Code\] \--\>|GitHub Actions| B(🔨 Build Docker Image)  
    B \--\> C{🛡️ Trivy Security Scan}  
    C \--\>|Pass| D\[📦 Push to GHCR\]  
    C \--\>|Fail| X\[❌ Stop Pipeline\]  
    D \--\> E\[🚀 Deploy to Minikube\]  
    E \--\> F\[🌐 Expose via NodePort\]

### **Pipeline Stages**

1. **Semantic Versioning:** Automatically determines the next version tag (e.g., v1.0.1) based on commit messages.  
2. **Containerization:** Builds a lightweight, optimized Docker image for the Node.js application.  
3. **Vulnerability Scanning:** Uses **Trivy** to scan the image for CRITICAL and HIGH CVEs. *The build fails immediately if risks are detected.*  
4. **Artifact Management:** Securely pushes the verified image to the **GitHub Container Registry (GHCR)**.  
5. **Kubernetes Deployment:** Updates the deployment manifest and performs a rolling update on the Minikube cluster.

## **🛠️ Technology Stack**

| Component | Technology | Description |
| :---- | :---- | :---- |
| **Source Control** |  | Version control and repository management. |
| **CI/CD** |  | Automates the build, scan, and deploy workflow. |
| **Containerization** |  | Container runtime and image building. |
| **Orchestration** |  | Manages containerized application deployment. |
| **Security** |  | Scans container images for security vulnerabilities. |
| **Backend** |  | Lightweight Express.js application. |
| **Environment** |  | Hosting environment for the Self-Hosted Runner. |

## **🚀 Getting Started**

To replicate this environment locally, follow these steps.

### **1\. Prerequisites**

Ensure your environment (VM or Local Machine) has the following installed:

* Docker & Docker Compose  
* Minikube  
* Kubectl  
* Git

### **2\. Installation**

\# Clone the repository  
git clone \[https://github.com/RoshaneAnees/devsecops.git\](https://github.com/RoshaneAnees/devsecops.git)  
cd devsecops

\# Start Minikube  
minikube start \--driver=docker

\# Check Cluster Status  
kubectl get nodes

### **3\. Deployment**

The deployment is handled automatically by the GitHub Actions pipeline. However, you can apply manifests manually for testing:

kubectl apply \-f k8s/

## **🔒 Security Implementation**

Security is the core component of this project. The pipeline integrates **Aqua Security's Trivy** scanner.

* **Scanned Target:** ghcr.io/roshaneanees/devsecops-app:latest  
* **Severity Threshold:** CRITICAL, HIGH  
* **Action:** If any vulnerability matching these severities is found, the pipeline **halts immediately**, preventing insecure code from reaching production.

## **🌐 Accessing the Application**

Once the pipeline completes successfully:

1. **Verify the Pods are running:**  
   kubectl get pods

2. **Access the Service:**  
   \# Get the URL directly from Minikube  
   minikube service devsecops-svc \--url

3. **Port Forwarding (Alternative):**  
   kubectl port-forward svc/devsecops-svc 3000:3000

   Visit http://localhost:3000 in your browser.

## **📂 Repository Structure**

devsecops/  
├── .github/workflows/  
│   └── cicd.yaml        \# ⚙️ The Brain: CI/CD Pipeline Definition  
├── app/  
│   ├── Dockerfile       \# 🐳 Instructions to build the image  
│   ├── package.json     \# 📦 Dependencies definition  
│   └── index.js         \# ⚡ Server Logic  
├── k8s/  
│   ├── deployment.yaml  \# ☸️ Deployment configuration  
│   └── service.yaml     \# 🌐 Network Exposure configuration  
└── README.md            \# 📄 Project Documentation

## **👨‍💻 Author**

\<div align="center"\>

**RoshaneAnees**

*DevSecOps Implementation Project • Fall 2026*

\</div\>