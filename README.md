This is the kind of project that makes recruiters say:

> “Okay, this candidate understands infrastructure + automation.”

We will structure this as:

* 🧠 Architecture (Cloud-level thinking)
* 🏗 Terraform (Provision Infra)
* ⚙ Ansible (Configure Server)
* 🐳 Docker Deployment
* 🌐 Nginx Reverse Proxy
* 🔐 Security
* 🚀 CI/CD Integration
* 🎯 Interview Positioning

---

# 🚀 Project Title

**Production-Ready 3-Tier Application Deployment on AWS EC2 using Terraform, Ansible, Docker & Nginx Reverse Proxy**

---

# 1️⃣ Cloud Architecture Design

## 📐 Draw This in Eraser.io

```text
                User (Browser)
                      |
                      v
                Public IP (EC2)
                      |
                      v
             Nginx Reverse Proxy
                      |
                      v
               Docker Network
          --------------------------
          |         |             |
      Frontend   Backend       MySQL
      Container  Container     Container
                      |
                      v
                   Volume
```

---

# 🧠 DevOps Thinking (Very Important)

We are separating responsibilities:

| Layer             | Tool           | Responsibility             |
| ----------------- | -------------- | -------------------------- |
| Infra Provision   | Terraform      | Create EC2, Security Group |
| Configuration     | Ansible        | Install Docker, Nginx      |
| App Packaging     | Docker         | Containerization           |
| App Orchestration | Docker Compose | Multi-tier management      |
| Traffic Routing   | Nginx          | Reverse proxy              |
| Automation        | CI/CD          | Auto deployment            |

This separation is what defines a DevOps Engineer.

---

# 2️⃣ Terraform – Infrastructure Provisioning

## 🎯 Goal

Provision:

* EC2 Instance (Ubuntu)
* Security Group (Allow 22, 80, 443)
* Key Pair

---

## 📁 Terraform Folder Structure

```text
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

---

## 🔹 main.tf

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_security_group" "app_sg" {
  name = "app-security-group"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "app_server" {
  ami           = "ubuntu-ami-id"
  instance_type = "t2.micro"
  security_groups = [aws_security_group.app_sg.name]
  key_name = "your-key"

  tags = {
    Name = "3tier-docker-server"
  }
}
```

---

## 🔹 Commands

```bash
terraform init
terraform plan
terraform apply
```

---

# 3️⃣ Ansible – Server Configuration

## 🎯 Goal

After EC2 creation:

* Install Docker
* Install Docker Compose
* Install Nginx
* Clone GitHub repo
* Run docker-compose

---

## 📁 Ansible Structure

```text
ansible/
├── inventory.ini
├── playbook.yml
```

---

## 🔹 inventory.ini

```ini
[app]
<EC2_PUBLIC_IP> ansible_user=ubuntu ansible_ssh_private_key_file=key.pem
```

---

## 🔹 playbook.yml

```yaml
- hosts: app
  become: yes
  tasks:

    - name: Install dependencies
      apt:
        name: ['docker.io', 'nginx', 'git']
        state: present
        update_cache: yes

    - name: Start Docker
      service:
        name: docker
        state: started
        enabled: yes

    - name: Clone repo
      git:
        repo: https://github.com/yourrepo/3tier-app.git
        dest: /home/ubuntu/app

    - name: Run docker-compose
      command: docker compose up -d
      args:
        chdir: /home/ubuntu/app
```

---

# 4️⃣ Nginx Reverse Proxy (Production Pattern)

Instead of exposing backend directly:

User → Nginx → Backend

---

## 🔹 Nginx Config

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://frontend:80;
    }

    location /api/ {
        proxy_pass http://backend:5000;
    }
}
```

---

## Why Reverse Proxy?

* Hide backend ports
* SSL termination
* Load balancing
* Security layer

Interview Gold Question:

> Why not expose backend directly?

Answer:
Because it increases attack surface and breaks layered architecture.

---

# 5️⃣ Security Improvements (DevOps Maturity)

## 🔐 Add:

* Use IAM Role instead of AWS keys
* Restrict SSH to your IP
* Use Let's Encrypt SSL
* Run containers as non-root
* Use .dockerignore
* Add resource limits
* Use Docker secrets (production)

---

# 6️⃣ CI/CD Integration (GitHub Actions Example)

## Flow:

```text
Developer → Push Code
       ↓
GitHub Actions
       ↓
Build Docker Image
       ↓
Push to Docker Hub
       ↓
SSH to EC2
       ↓
Pull latest image
       ↓
docker-compose up -d
```

---

# Sample GitHub Action

```yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - run: docker build -t username/app .
      - run: docker push username/app
```

---

# 7️⃣ Full DevOps Thinking Breakdown

### Infrastructure as Code

Manual EC2 creation = ❌
Terraform = ✅ reproducible infra

### Configuration as Code

Manual SSH install = ❌
Ansible = ✅ automated configuration

### Immutable Infrastructure

No manual edits inside EC2.

### Separation of Concerns

Infra ≠ App ≠ Config

### Idempotency

Ansible ensures repeated runs do not break system.

---

# 8️⃣ How You Explain This in Interview (Important)

“I designed a production-style 3-tier application.
Terraform provisions infrastructure.
Ansible configures server.
Docker handles containerization.
Nginx works as reverse proxy.
CI/CD automates deployment.
All components are isolated using Docker networks and volumes.”

That answer alone shows maturity.

---

# 9️⃣ Advanced Upgrade (Optional Next Step)

* Move to Auto Scaling Group
* Add Application Load Balancer
* Store images in AWS ECR
* Replace Docker Compose with Kubernetes
* Add Prometheus + Grafana monitoring

---

# 🔥 Why This Project Is Powerful For You

Considering:

* You want DevOps
* You want DevSecOps
* You already use AWS
* You are learning Docker

This becomes your flagship DevOps showcase project.

---

