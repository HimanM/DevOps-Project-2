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

## 3. Install Docker & Docker Compose
The pipeline builds Docker images, so Jenkins needs Docker.
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add Jenkins user to docker group
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

## 4. Configure Jenkins
1.  **Unlock Jenkins**: Get the initial password from `/var/lib/jenkins/secrets/initialAdminPassword`.
2.  **Install Plugins**: Select "Install suggested plugins". Also install "Docker Pipeline" and "SSH Agent" plugins.
3.  **Create Admin User**: Follow the prompts.

## 5. Set up Credentials
Go to **Manage Jenkins > Credentials > System > Global credentials**.

### DockerHub
- **Kind**: Username with password
- **ID**: `dockerhub-username` (Username) / `dockerhub-token` (Password/Token)
- **Description**: DockerHub Credentials

### VPS SSH Key (For Deployment)
- **Kind**: SSH Username with private key
- **ID**: `vps-ssh-key`
- **Username**: Your VPS username (e.g., `root` or `ubuntu`)
- **Private Key**: Enter the private key directly.

## 6. Create Pipelines
1.  **New Item** > **Pipeline**.
2.  **Definition**: Pipeline script from SCM.
3.  **SCM**: Git.
4.  **Repository URL**: Your GitHub Repo URL.
5.  **Script Path**: `Jenkinsfile` (for CI) or `Jenkinsfile.deploy` (for CD).
