# 🚀 DevOps-Project-ZOMATO-Application-Deployment

## 📌 Project Overview

This project demonstrates a comprehensive implementation of the DevOps lifecycle for deploying a ZOMATO Clone application, utilizing modern DevOps, DevSecOps, Monitoring, and Kubernetes practices.

The solution includes:

- CI/CD automation using Jenkins  
- Code quality and security scanning  
- Containerization with Docker  
- Continuous monitoring using Prometheus & Grafana  
- Kubernetes deployment using Amazon EKS  
- GitOps-based deployment with Argo CD 

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🧰 Technology Stack

| Category | Tools |
|--------|------|
| Cloud | AWS (EC2, EKS, IAM, CloudFormation) |
| CI/CD | Jenkins |
| Containerization | Docker |
| Code Quality | SonarQube |
| Security | Trivy, OWASP Dependency-Check, Docker Scout |
| Monitoring | Prometheus, Grafana, Node Exporter |
| Orchestration | Kubernetes (EKS) |
| GitOps | Argo CD |

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  ⚙️ Infrastructure Setup
### 1️⃣ Launch Jenkins Server (CI/CD VM)

  - OS: Ubuntu 24.04
  - Instance Type: t2.large
  - Storage: 30 GB
  - Ports: 22, 8080, 9000, 3000
