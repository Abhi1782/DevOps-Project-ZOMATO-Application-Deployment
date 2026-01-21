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

### 2️⃣ System Preparation

    sudo su
    sudo apt update -y

### 3️⃣ Install AWS CLI

    sudo apt install unzip -y
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
    unzip awscliv2.zip
    sudo ./aws/install

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🔧 Jenkins Installation

    #!/bin/bash
    sudo apt update -y
    wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
    echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee             /etc/apt/sources.list.d/adoptium.list
    sudo apt update -y
    sudo apt install temurin-17-jdk -y
    /usr/bin/java --version
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install jenkins -y
    sudo systemctl start jenkins
    sudo systemctl status jenkins

  - Access Jenkins: http://<EC2-IP>:8080
  - Complete initial Jenkins setup via UI

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🐳 Docker Installation

    sudo apt-get update
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
    # Add the repository to Apt sources:
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    sudo usermod -aG docker ubuntu
    sudo chmod 777 /var/run/docker.sock
    newgrp docker
    sudo systemctl status docker

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  🔐 Security Tools Setup

### 🔹 Trivy Installation

    sudo apt-get install wget apt-transport-https gnupg
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
    sudo apt-get update
    sudo apt-get install trivy

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

🔹 Docker Scout

   - Login to DockerHub via browser
   - Enable Docker Scout

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📊 SonarQube Setup

    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

  - docker ps (You can see the SonarQube container)
  - Access: http://<EC2-IP>:9000
  - Configure SonarQube token and webhook in Jenkins

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  🔌 Jenkins Plugin Configuration

### Install the following plugins:

  - SonarQube Scanner
  - OWASP Dependency-Check
  - Docker Pipeline
  - Email Extension
  - NodeJS
  - Prometheus Metrics

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  🔁 CI/CD Pipeline

### 📜 Jenkins Pipeline Features

  - Git Checkout
  - SonarQube Analysis & Quality Gate
  - OWASP Dependency Scan
  - Trivy FS Scan
  - Docker Image Build & Push
  - Docker Scout Analysis
  - Application Deployment
  - Email Notifications

📌 Note:
Replace:

DockerHub username
Email ID
Credentials IDs
before running the pipeline.
