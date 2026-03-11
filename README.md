#  Jenkins Pipeline Triggering Ansible Playbook using Ansible Master–Worker Architecture

##  Project Overview

This project demonstrates how to configure **Jenkins and Ansible in a Master–Worker architecture**.
Jenkins runs on the **master node**, and Ansible manages the **worker node** through SSH.

The Jenkins pipeline will later trigger an **Ansible Playbook** to automate configuration on the worker server.

---

#  Architecture

```
GitHub Repository
        │
        ▼
   Jenkins Pipeline
        │
        ▼
   Ansible Master Node
        │
   (SSH Connection)
        │
        ▼
   Worker Node
```

---

#  System Requirements

| Component     | Requirement     |
| ------------- | --------------- |
| Cloud         | AWS EC2         |
| OS            | Ubuntu 24.04    |
| Instances     | 2               |
| Instance Type | t3.small        |
| Ports         | 22, 8080        |
| Terminal      | MobaXterm / SSH |

---

# Step 1: Launch EC2 Instances

Create **2 servers**

### Master Node

* Jenkins
* Ansible

### Worker Node

* Target machine managed by Ansible

---

#  Step 2: Install Ansible on Master Node

Update packages

```bash
sudo apt update
```

Install Java

```bash
sudo apt install openjdk-17-jre-headless -y
```

Install Ansible

```bash
bash <(curl -sL https://tinyurl.com/mse8n4k6)
```

Verify installation

```bash
ansible --version
```

---

#  Step 3: Create Ansible User

```bash
sudo adduser ansible
sudo usermod -aG sudo ansible
```

Switch to ansible user

```bash
su - ansible
```

---

#  Step 4: Generate SSH Key

Generate SSH key from **Master Node**

```bash
ssh-keygen
```

Press **Enter three times**

Public key location

```
~/.ssh/id_rsa.pub
```

---

#  Step 5: Configure Worker Node

Login to **Worker Node**

Update packages

```bash
sudo apt update
```

Create ansible user

```bash
sudo adduser ansible
```

Add master public key

```bash
sudo vi /home/ansible/.ssh/authorized_keys
```

Restart SSH service

```bash
sudo systemctl restart ssh
```

---

#  Step 6: Test SSH Connection

From master node

```bash
ssh ansible@<worker-private-ip>
```

---

#  Step 7: Configure Ansible Inventory

Edit hosts file

```bash
sudo vi /etc/ansible/hosts
```

Add worker node

```
[dev]
<worker-private-ip>
```

Test connection

```bash
ansible dev -m ping
```

Expected output

```
SUCCESS => pong
```

---

# Step 8: Install Jenkins on Master Node

Add Jenkins key

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

Add Jenkins repository

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list
```

Update packages

```bash
sudo apt update
```

Install Jenkins

```bash
sudo apt install jenkins -y
```

Start Jenkins

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

---

#  Step 9: Access Jenkins

Open browser

```
http://<MASTER_PUBLIC_IP>:8080
```

Get Jenkins password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Install **Suggested Plugins** and create **Admin User**.

---

#  Step 10: Give Jenkins Sudo Access

```bash
sudo visudo
```

Add the following line

```
jenkins ALL=(ALL) NOPASSWD:ALL
```

---

#  Project Structure

```
ansible-jenkins-project
│
├── Jenkinsfile
└── nginx-playbook.yml
```

---

#  Tools Used

* Jenkins
* Ansible
* GitHub
* AWS EC2
* Ubuntu 24.04

---

#  Author

**DevOps Automation Project**
Jenkins + Ansible CI/CD Pipeline
<img width="1920" height="1080" alt="Screenshot (91)" src="https://github.com/user-attachments/assets/5b04a134-5b35-427c-af02-937d4ad56a14" />
<img width="1920" height="1080" alt="Screenshot (90)" src="https://github.com/user-attachments/assets/2ec5c3e1-af57-488d-8145-b013867c76f6" />


