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

![DevOps Project ZOMATO Application Clone](https://github.com/user-attachments/assets/c20965ab-09b4-4156-966d-a3e8c3d73b9d)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  ⚙️ Infrastructure Setup

### 1️⃣ Launch Jenkins Server (CI/CD VM)

  - OS: Ubuntu 24.04
  - Instance Type: t2.large
  - Storage: 30 GB
  - Ports: 22, 8080, 9000, 3000

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (109)" src="https://github.com/user-attachments/assets/ebd51e48-54f6-4e80-b9c7-9bb2f5b20014" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 2️⃣ System Preparation

    sudo su
    sudo apt update -y

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (110)" src="https://github.com/user-attachments/assets/063153b2-9649-40d9-a90a-b649836f61cf" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (111)" src="https://github.com/user-attachments/assets/9bc2be94-a6f6-4752-9c7c-bcfae983c599" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

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

<img width="1089" height="49" alt="Screenshot 2026-01-13 204548" src="https://github.com/user-attachments/assets/c85d9316-37f5-4c0b-b703-e2d6d4323d58" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (118)" src="https://github.com/user-attachments/assets/ae8792f1-07f1-4f55-aadd-5f3b51105bf6" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (112)" src="https://github.com/user-attachments/assets/a849c374-ae29-43bc-b9aa-5a16a09b809b" />

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

<img width="1077" height="53" alt="Screenshot 2026-01-21 220724" src="https://github.com/user-attachments/assets/6d0d7cb7-fbf1-47f1-8eee-ade9df6e927a" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (113)" src="https://github.com/user-attachments/assets/9154968a-e446-4f70-968c-c727b1d9f620" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  🔐 Security Tools Setup

### 🔹 Trivy Installation

    sudo apt-get install wget apt-transport-https gnupg
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
    sudo apt-get update
    sudo apt-get install trivy

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1083" height="53" alt="Screenshot 2026-01-13 204820" src="https://github.com/user-attachments/assets/8523c7ba-dff4-445a-abba-79f197ae75b5" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### 🔹 Docker Scout

   - Login to DockerHub via browser
   - Enable Docker Scout

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (127)" src="https://github.com/user-attachments/assets/ff44ec85-5d4c-4b67-80b9-5052b3edad55" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📊 SonarQube Setup

    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

  - docker ps (You can see the SonarQube container)
  - Access: http://<EC2-IP>:9000
  - Configure SonarQube token and webhook in Jenkins

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (114)" src="https://github.com/user-attachments/assets/6b6d8de3-ff48-4bf3-a091-a3fa350631e5" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (115)" src="https://github.com/user-attachments/assets/af1e8670-072b-412c-b256-1f40bdead755" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/05b25496-14b6-49bc-87d1-460174df646a" />

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

<img width="1366" height="768" alt="Screenshot (117)" src="https://github.com/user-attachments/assets/7aa63f34-38f5-4d3c-b593-556f8845dac0" />

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

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (121)" src="https://github.com/user-attachments/assets/4ded7db7-b0a6-4efc-adc6-a12021393f1e" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1354" height="724" alt="Screenshot (131)" src="https://github.com/user-attachments/assets/2c01b74b-44b6-43a6-af06-47d37a8d643c" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
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

<img width="1366" height="768" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/bd5606c2-765f-43f3-8ced-7a0f834b1d5d" />

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

<img width="1366" height="768" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/328ba4df-4d8c-4367-8d5e-bcdbd0fb509c" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Prometheus Installation

  - Dedicated Prometheus user
  - Systemd service configuration
  - Access: http://<Monitoring-IP>:9090

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

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

<img width="1366" height="768" alt="Screenshot (133)" src="https://github.com/user-attachments/assets/4848f31a-1df9-4cc3-80fa-5177faeb8d91" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (134)" src="https://github.com/user-attachments/assets/b3f0192e-cdbb-4962-b181-7d5aed3d7724" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/82c055a2-f8ff-4715-be91-dd94ff3f6a11" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (136)" src="https://github.com/user-attachments/assets/34d7ce90-4ef9-4da3-8862-dab98ba65f00" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Node Exporter

  - Installed as a system service
  - Scrapes node-level metrics
  - Port: 9100

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (137)" src="https://github.com/user-attachments/assets/698ba726-56da-4d39-87bf-8850c9109c34" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
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

### 🔹Configure Prometheus Plugin Integration Jenkins & Node Exporter Integration


    As of now, we created a Prometheus service, but we need to add a job in order to fetch the details by node exporter. So, for that, we need to create 2 jobs, one with 'node exporter'     and the other with 'jenkins' as shown below;
    
    Integrate Jenkins with Prometheus to monitor the CI/CD pipeline.
    
    Prometheus Configuration:
    
    To configure Prometheus to scrape metrics from Node Exporter and Jenkins, you need to modify the prometheus.yml file. 
    The path of prometheus.yml is: cd /etc/prometheus/ ----> ls -l ----> You can see the "prometheus.yml" file ----> sudo vi prometheus.yml ----> You will see the content and also there     is a default job called "Prometheus". Paste the below content at the end of the file;
    
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

<img width="1366" height="768" alt="Screenshot (138)" src="https://github.com/user-attachments/assets/e58bd3a4-a1bd-401e-b104-e4bf779d1b48" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📊 Grafana Installation

 - Port: 3000
 - Default login: admin / admin
 - Data Source: Prometheus
 - Dashboards added for:

     - Node Metrics
     - Jenkins Metrics

            You are currently in /etc/Prometheus path.
            
            Install Grafana on Monitoring Server;
            
            Step 1: Install Dependencies:
            First, ensure that all necessary dependencies are installed:
            sudo apt-get update
            sudo apt-get install -y apt-transport-https software-properties-common
            
            Step 2: Add the GPG Key:
            cd ---> You are now in ~ path
            Add the GPG key for Grafana:
            wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
            
            You should see OK when executed the above command.
            
            Step 3: Add Grafana Repository:
            Add the repository for Grafana stable releases:
            echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
            
            Step 4: Update and Install Grafana:
            Update the package list and install Grafana:
            sudo apt-get update
            sudo apt-get -y install grafana
            
            Step 5: Enable and Start Grafana Service:
            To automatically start Grafana after a reboot, enable the service:
            sudo systemctl enable grafana-server
            
            Start Grafana:
            sudo systemctl start grafana-server
            
            Step 6: Check Grafana Status:
            Verify the status of the Grafana service to ensure it's running correctly:
            sudo systemctl status grafana-server
            
            You should see "Active (running)" in green colour
            Press control+c to come out
            
            Step 7: Access Grafana Web Interface:
            The default port for Grafana is 3000
            http://<monitoring-server-ip>:3000
            
            Default id and password is "admin"
            You can Set new password or you can click on "skip now".
            Click on "skip now" (If you want you can create the password)
            
            You will see the Grafana dashboard
            
            The first thing that we have to do in Grafana is to add the data source
            Lets add the data source;
            
            Click on Dashboards in the left pane, you can see both the dashboards you have just added.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (139)" src="https://github.com/user-attachments/assets/e5c2406c-9084-447b-9f26-4e8485ab3a8d" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (142)" src="https://github.com/user-attachments/assets/3d775777-5055-46f3-8759-b988f5fe5a33" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (143)" src="https://github.com/user-attachments/assets/5064a504-c1e4-410d-a75f-df211b1481b8" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (146)" src="https://github.com/user-attachments/assets/36779cdc-f98f-44e5-a14b-32d8724bd840" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (150)" src="https://github.com/user-attachments/assets/313126b6-df17-482d-aa05-f14f8ce0c8dc" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  ☸️ Minikube Setup and Zomato Application Deployment on Kubernetes

### 🔹 Step 1: Install kubectl

        curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
        chmod +x kubectl
        sudo mv kubectl /usr/local/bin/

Verify:

    kubectl version --client

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 2: Install Minikube
    
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube

Verify:
      
      minikube version

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1074" height="64" alt="Screenshot 2026-01-21 223621" src="https://github.com/user-attachments/assets/68a3b940-452f-447d-8cf7-ebc8555c1c24" />

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 3: Start Minikube on Server

Start Minikube using Docker driver:

    minikube start --driver=docker

Verify cluster status:

    kubectl get nodes
    kubectl cluster-info

✔ Node should be in Ready state.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (172)" src="https://github.com/user-attachments/assets/7b12666c-d422-44a9-93b9-41bd25419fad" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 4: Build or Load Zomato Docker Image

Option 1: Build image inside Minikube

    eval $(minikube docker-env)
    docker build -t zomato-app:latest .

Option 2: Load existing image

    minikube image load zomato-app:latest

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 5: Create Kubernetes Deployment Manifest

- deployment.yaml

      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: zomato-deployment
      spec:
        replicas: 2
        selector:
          matchLabels:
            app: zomato
        template:
          metadata:
            labels:
              app: zomato
          spec:
            containers:
            - name: zomato-container
              image: zomato-app:latest
              imagePullPolicy: IfNotPresent
              ports:
              - containerPort: 80

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 6: Create Kubernetes Service Manifest

- service.yaml

      apiVersion: v1
      kind: Service
      metadata:
        name: zomato-service
      spec:
        type: NodePort
        selector:
          app: zomato
        ports:
          - port: 80
            targetPort: 80
            nodePort: 30007

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 ### 🔹 Step 7: Deploy Zomato Application

  Apply the Kubernetes manifests:

      kubectl apply -f deployment.yaml
      kubectl apply -f service.yaml

  Verify resources:

      kubectl get deployments
      kubectl get pods
      kubectl get svc

✔ Pods should be Running.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1082" height="96" alt="Screenshot 2026-01-21 222707" src="https://github.com/user-attachments/assets/ae56f6dc-0f30-4843-9082-0918cec0d7cd" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### 🔹 Step 8: Access the Application

- Get Minikube IP:

      minikube ip

- Access the application:

      http://<MINIKUBE-IP>:30007

✔ Zomato application is now live on Kubernetes.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="1366" height="768" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/8f73660a-094f-4284-a1b4-e24404a2b4fe" />

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 9: Scaling the Application

Scale the deployment horizontally:

    kubectl scale deployment zomato-deployment --replicas=3

Verify:

    kubectl get pods

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🔹 Step 10: Logs and Validation

Check pod logs:

    kubectl logs <pod-name>

Describe pod:

    kubectl describe pod <pod-name>

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                          
<p align="center">
  <strong>HAPPY LEARNING</strong><br>
  <em>© Abhishek Prajapati | DevOps & Cloud Engineer</em><br>
  <sub>Docker • Kubernetes • AWS • CI/CD</sub>
</p>
