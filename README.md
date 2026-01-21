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

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

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

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🏗️ Architecture Flow

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

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

- DockerHub username
- Email ID
- Credentials IDs
before running the pipeline.

      *********************
      Pipeline Script
      *********************
           pipeline {
          agent any
      
          tools {
              jdk 'jdk21'
              nodejs 'node23'
          }
      
          environment {
              SCANNER_HOME = tool 'sonar-scanner'
          }
      
          stages {
      
              stage('Clean Workspace') {
                  steps {
                      cleanWs()
                  }
              }
      
              stage('Git Checkout') {
                  steps {
                      git 'https://github.com/Abhi1782/DevOps-Project-Zomato-Kastro.git'
                  }
              }
      
              stage('SonarQube Analysis') {
                  steps {
                      withSonarQubeEnv('sonar-server') {
                          sh '''
                              $SCANNER_HOME/bin/sonar-scanner \
                              -Dsonar.projectName=zomato \
                              -Dsonar.projectKey=zomato
                          '''
                      }
                  }
              }
      
              stage('Quality Gate') {
                  steps {
                      script {
                          waitForQualityGate abortPipeline: false
                      }
                  }
              }
      
              stage('Install NPM Dependencies') {
                  steps {
                      sh 'npm install'
                  }
              }
      
              stage('OWASP Dependency Check') {
                  steps {
                      dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit -n',
                                      odcInstallation: 'DP-Check'
                      dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                  }
              }
      
              stage('Trivy File Scan') {
                  steps {
                      sh 'trivy fs . > trivy.txt'
                  }
              }
      
              stage('Build Docker Image') {
                  steps {
                      sh 'docker build -t zomato .'
                  }
              }
      
              stage('Tag & Push Docker Image') {
                  steps {
                      script {
                          withDockerRegistry(credentialsId: 'docker') {
                              sh '''
                                  docker tag zomato abhiprajapati504/zomato:latest
                                  docker push abhiprajapati504/zomato:latest
                              '''
                          }
                      }
                  }
              }
      
              stage('Docker Scout Scan') {
                  steps {
                      sh '''
                          docker scout quickview abhiprajapati504/zomato:latest
                          docker scout cves abhiprajapati504/zomato:latest
                          docker scout recommendations abhiprajapati504/zomato:latest
                      '''
                  }
              }
      
              stage('Deploy to Container') {
                  steps {
                      sh '''
                          docker stop zomato || true
                          docker rm zomato || true
                          docker run -d \
                            --name zomato \
                            -p 3000:3000 \
                            --restart unless-stopped \
                            abhiprajapati504/zomato:latest
                      '''
                  }
              }
          }
      
          post {
              always {
                  emailext(
                      attachLog: true,
                      subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME}",
                      body: """
                      <html>
                      <body>
                          <h3>Pipeline Result</h3>
                          <p><b>Job:</b> ${env.JOB_NAME}</p>
                          <p><b>Build:</b> ${env.BUILD_NUMBER}</p>
                          <p><b>URL:</b> ${env.BUILD_URL}</p>
                      </body>
                      </html>
                      """,
                      to: 'abhishek.prajapati1782@gmail.com',
                      mimeType: 'text/html',
                      attachmentsPattern: 'trivy.txt'
                  )
              }
          }
      }

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## ⚠️ OWASP UNSTABLE Fix

If OWASP FS Scan shows UNSTABLE, replace the scan stage with:

    dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit --update -n'

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  📈 Monitoring Setup

### 2️⃣ Monitoring Server (New EC2)

  - Name: Monitoring Server
  - OS: Ubuntu 24.04
  - Instance Type: t2.large
  - Storage: 30 GB
  - Ports: 9090, 9100, 3000

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

###🔹 Prometheus Installation

  - Dedicated Prometheus user
  - Systemd service configuration
  - Access: http://<Monitoring-IP>:9090

        First, create a dedicated Linux user for Prometheus and download Prometheus
        sudo useradd --system --no-create-home --shell /bin/false prometheus
        wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
        
        Extract Prometheus files, move them, and create directories:
        tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
        cd prometheus-2.47.1.linux-amd64/
        sudo mkdir -p /data /etc/prometheus
        sudo mv prometheus promtool /usr/local/bin/
        sudo mv consoles/ console_libraries/ /etc/prometheus/
        sudo mv prometheus.yml /etc/prometheus/prometheus.yml
        
        Set ownership for directories:
        sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
        
        Create a systemd unit configuration file for Prometheus:
        sudo vi /etc/systemd/system/prometheus.service
        
        Add the following content to the prometheus.service file:
        [Unit]
        Description=Prometheus
        Wants=network-online.target
        After=network-online.target
        
        StartLimitIntervalSec=500
        StartLimitBurst=5
        
        [Service]
        User=prometheus
        Group=prometheus
        Type=simple
        Restart=on-failure
        RestartSec=5s
        ExecStart=/usr/local/bin/prometheus \
          --config.file=/etc/prometheus/prometheus.yml \
          --storage.tsdb.path=/data \
          --web.console.templates=/etc/prometheus/consoles \
          --web.console.libraries=/etc/prometheus/console_libraries \
          --web.listen-address=0.0.0.0:9090 \
          --web.enable-lifecycle
        
        [Install]
        WantedBy=multi-user.target
        
        Explanation of the key elements in the above prometheus.service file:
        User and Group specify the Linux user and group under which Prometheus will run.
        ExecStart is where you specify the Prometheus binary path, the location of the configuration file (prometheus.yml), the storage directory, and other settings.
        web.listen-address configures Prometheus to listen on all network interfaces on port 9090.
        web.enable-lifecycle allows for management of Prometheus through API calls.
        
        
        Enable and start Prometheus:
        sudo systemctl enable prometheus
        sudo systemctl start prometheus
        
        Verify Prometheus's status:
        sudo systemctl status prometheus
        
        Press Control+c to come out
        
        Access Prometheus in browser using your server's IP and port 9090:
        http://<your-server-ip>:9090
        
        If it doesn't work, in the web link of browser, remove 's' in 'https'. Keep only 'http' and now you will be able to see.
        You can see the Prometheus console.
        Click on 'Status' dropdown ---> Click on 'Targets' ---> You can see 'Prometheus (1/1 up)'
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Node Exporter

  - Installed as a system service
  - Scrapes node-level metrics
  - Port: 9100

        cd 
        You are in ~ path now
        
        Create a system user for Node Exporter and download Node Exporter:
        sudo useradd --system --no-create-home --shell /bin/false node_exporter
        wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
        
        Extract Node Exporter files, move the binary, and clean up:
        tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
        sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
        rm -rf node_exporter*
        
        Create a systemd unit configuration file for Node Exporter:
        sudo vi /etc/systemd/system/node_exporter.service
        
        Add the following content to the node_exporter.service file:
        [Unit]
        Description=Node Exporter
        Wants=network-online.target
        After=network-online.target
        
        StartLimitIntervalSec=500
        StartLimitBurst=5
        
        [Service]
        User=node_exporter
        Group=node_exporter
        Type=simple
        Restart=on-failure
        RestartSec=5s
        ExecStart=/usr/local/bin/node_exporter --collector.logind
        
        [Install]
        WantedBy=multi-user.target
        
        Note: Replace --collector.logind with any additional flags as needed.
        
        Enable and start Node Exporter:
        sudo systemctl enable node_exporter
        sudo systemctl start node_exporter
        
        Verify the Node Exporter's status:
        sudo systemctl status node_exporter
        You can see "active (running)" in green colour
        Press control+c to come out of the file

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

###🔹Configure Prometheus Plugin Integration Jenkins & Node Exporter Integration

As of now, we created a Prometheus service, but we need to add a job in order to fetch the details by node exporter. So, for that, we need to create 2 jobs, one with 'node exporter' and the other with 'jenkins' as shown below;

Integrate Jenkins with Prometheus to monitor the CI/CD pipeline.

Prometheus Configuration:

To configure Prometheus to scrape metrics from Node Exporter and Jenkins, you need to modify the prometheus.yml file. 
The path of prometheus.yml is: cd /etc/prometheus/ ----> ls -l ----> You can see the "prometheus.yml" file ----> sudo vi prometheus.yml ----> You will see the content and also there is a default job called "Prometheus". Paste the below content at the end of the file;

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['<MonitoringVMip>:9100']

  - job_name: 'jenkins'
    metrics_path: '/prometheus'
    static_configs:
      - targets: ['<your-jenkins-ip>:<your-jenkins-port>']

 In the above, replace <your-jenkins-ip> and <your-jenkins-port> with the appropriate IPs ----> esc ----> :wq

Check the validity of the configuration file:
promtool check config /etc/prometheus/prometheus.yml

You should see "SUCCESS" when you run the above command; it means every configuration made so far is good.

Reload the Prometheus configuration without restarting:
curl -X POST http://localhost:9090/-/reload

Access Prometheus in browser (if already opened, just reload the page):
http://<your-prometheus-ip>:9090/targets

Open Port number 9100 for Monitoring VM 

You should now see "Jenkins (1/1 up)", "node exporter (1/1 up)" and "prometheus (1/1 up)" in the Prometheus browser.
Click on "showmore" next to "Jenkins." You will see a link. Open the link in a new tab to see the metrics that are getting scraped

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
