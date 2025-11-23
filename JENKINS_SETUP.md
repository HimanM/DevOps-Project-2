# Jenkins Setup Guide on VPS

This guide explains how to set up Jenkins on your VPS to run the CI/CD pipelines.

## Prerequisites
- A VPS (Ubuntu 20.04/22.04 recommended).
- Root or sudo access.

## 1. Install Java (Jenkins Dependency)
Jenkins requires Java to run.
```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

## 2. Install Jenkins
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```
Access Jenkins at `http://<VPS_IP>:8080`.

## 3. Install Docker, Docker Compose & sshpass
The pipeline builds Docker images and deploys via SSH using password.
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg sshpass
# Install Docker (Standard Docker installation)
sudo apt-get install docker.io docker-compose
```

### Important: Grant Jenkins Docker Permissions
To allow Jenkins to build Docker images, you must add the `jenkins` user to the `docker` group:
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

## 4. Install Required Plugins
1. Go to **Manage Jenkins > Plugins > Available plugins**.
2. Search for and install:
   - **Docker**
   - **Docker Pipeline**
3. Restart Jenkins after installation.

## 5. Set up Credentials
Go to **Manage Jenkins > Credentials > System > Global credentials**.

### DockerHub
- **Kind**: Username with password
- **ID**: `dockerhub-username`
- **Description**: DockerHub Credentials
> **Note**: If using a Docker Hub Access Token as the password, ensure it has **Read & Write** permissions.

### VPS Credentials (For Deployment)
- **Kind**: Username with password
- **ID**: `vps-credentials`
- **Username**: Your VPS username (e.g., `root` or `ubuntu`)
- **Password**: Your VPS password.
- **Description**: VPS Login Credentials

### VPS IP Address
- **Kind**: Secret text
- **ID**: `vps-ip`
- **Secret**: The IP address of your VPS (e.g., `192.168.1.100`)
- **Description**: VPS IP Address

## 6. Create Pipelines
1.  **New Item** > **Pipeline**.
2.  **Definition**: Pipeline script from SCM.
3.  **SCM**: Git.
4.  **Repository URL**: Your GitHub Repo URL.
5.  **Script Path**: `Jenkinsfile` (for CI) or `Jenkinsfile.deploy` (for CD).
