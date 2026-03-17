# 🚀 Production-Ready Jenkins CI/CD Pipeline (DevSecOps)

This repository contains a **production-ready Jenkins pipeline (Jenkinsfile)** integrating DevOps + DevSecOps tools:

* 🔐 Gitleaks (Secrets Detection)
* 📊 SonarQube (Code Quality)
* 🐳 Docker (Containerization)
* 🔍 Trivy (Image Security Scan)
* ☸️ Kubernetes (Deployment via manifests)
* 🛡️ Checkov (K8s Security Scan)

---

## 📌 📂 Project Structure

```
.
├── Jenkinsfile
├── Dockerfile
├── k8s-manifests/
│   ├── frontend_deployment.yaml
│   └── service.yaml
```

---

## ⚙️ 🔧 Prerequisites

Ensure the following tools are installed on your Jenkins server (agent node):

| Tool      | Purpose                  |
| --------- | ------------------------ |
| Jenkins   | CI/CD automation         |
| Docker    | Build & push images      |
| kubectl   | Kubernetes interaction   |
| Gitleaks  | Secret scanning          |
| SonarQube | Code quality analysis    |
| Trivy     | Image vulnerability scan |
| Checkov   | Kubernetes security scan |

---

## 🛠️ Installation Guide

### 1️⃣ Install Jenkins

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y

wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt update
sudo apt install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

---

### 2️⃣ Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

sudo usermod -aG docker jenkins
```

---

### 3️⃣ Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

---

### 4️⃣ Install Gitleaks

```bash
wget https://github.com/gitleaks/gitleaks/releases/latest/download/gitleaks-linux-amd64
chmod +x gitleaks-linux-amd64
sudo mv gitleaks-linux-amd64 /usr/local/bin/gitleaks
```

---

### 5️⃣ Install Trivy

```bash
sudo apt install wget apt-transport-https gnupg lsb-release -y

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list

sudo apt update
sudo apt install trivy -y
```

---

### 6️⃣ Install Checkov

```bash
pip install checkov
```

---

### 7️⃣ Setup SonarQube

Run SonarQube using Docker:

```bash
docker run -d --name sonarqube -p 9000:9000 sonarqube:lts
```

Access:

```
http://<your-server-ip>:9000
```

Default login:

```
admin / admin
```

---

## 🔑 Jenkins Configuration

### 🔐 Add Credentials

Go to:

```
Manage Jenkins → Credentials
```

Add:

| ID                       | Type              | Usage         |
| ------------------------ | ----------------- | ------------- |
| github                   | Username/Password | Clone code    |
| dockerhub-credentials-ID | Username/Password | Push image    |
| git-cred-ID              | Username/Password | Push manifest |

---

### ⚙️ Configure Tools

```
Manage Jenkins → Global Tool Configuration
```

* Add **Sonar Scanner**
* Name: `sonar-scanner`

---

### 🔗 Configure SonarQube

```
Manage Jenkins → Configure System
```

* Add SonarQube server
* Name: `SonarQube`
* Add token

---

## 🚀 Pipeline Workflow

1. 📥 Checkout source code
2. 🔐 Scan secrets using Gitleaks
3. 📊 Run SonarQube analysis
4. 🐳 Build Docker image
5. 🔍 Scan image using Trivy
6. 📦 Push image to Docker Hub
7. ☸️ Update Kubernetes manifests
8. 🛡️ Scan manifests using Checkov
9. 🔄 Push updated manifests
10. 🧹 Cleanup old Docker images

---

## 🧪 Running the Pipeline

1. Create a new **Pipeline Job** in Jenkins
2. Select:

```
Pipeline script from SCM
```

3. Add your GitHub repo URL
4. Select branch: `main`
5. Save & Click **Build Now**

---

## 🔐 Security Best Practices

* Never hardcode secrets (use Jenkins credentials)
* Enable Trivy + Gitleaks in pipeline
* Use private Docker repositories in production
* Enable RBAC in Kubernetes
* Rotate credentials regularly

---

## 🧠 Improvements (Future Scope)

* Add Helm instead of raw manifests
* Replace Ingress with Gateway API (future-ready)
* Integrate ArgoCD for GitOps deployment
* Add Slack/Email notifications
* Add OWASP Dependency Check

---

## 👨‍💻 Author

**Rahul Chaudhari**
DevOps Engineer | Cloud Enthusiast

---

## ⭐ Support

If you found this useful, give it a ⭐ on GitHub!
