# 🚀 EKS-Social-media-app-Project

This project demonstrates how to **build and deploy a Social Media application** to an **Amazon EKS cluster** using a **Jenkins CI/CD pipeline**.  
The setup integrates **Terraform** for infrastructure provisioning, **Kubernetes RBAC** for secure access control, and **Jenkins pipelines** for continuous integration and delivery.

---

## 🧱 Project Overview

1. **EKS Cluster Creation (Terraform)**
   - The project begins by provisioning an **Amazon EKS cluster** using **Terraform**.
   - Terraform automates the creation of EKS, node groups, VPCs, subnets, and IAM roles.
   - Once the cluster is created, `kubectl` and `aws-auth` configurations are applied.

2. **Continuous Integration (Jenkins CI)**
   - After cluster setup, a **Jenkins pipeline** was created to handle **Continuous Integration**.
   - This pipeline automates:
     - Source code fetching from GitHub.
     - Building and testing the application.
     - Creating Docker images and pushing them to ECR/Docker Hub.

3. **Kubernetes RBAC Configuration**
   - To securely allow Jenkins to deploy applications to the EKS cluster, the following resources were created:
     - **ServiceAccount** → `jenkins`
     - **Role** → `app-role`
     - **RoleBinding** → binds the `app-role` to the `jenkins` ServiceAccount.
     - **Secret** → token for the ServiceAccount used for Jenkins authentication.
   - These were applied inside the `webapps` namespace.

4. **Continuous Deployment (Jenkins CD)**
   - A second Jenkins pipeline was developed for **Continuous Deployment**.
   - This pipeline uses the ServiceAccount token to access the Kubernetes cluster and deploy the built images.
   - It automatically updates Kubernetes Deployments, Services, and other manifests in EKS.

---

## 📁 Repository Structure

| File / Directory | Description |
|------------------|-------------|
| **Jenkins Pipelines/** | Contains Jenkins pipeline scripts for CI and CD stages. |
| **Source code/** | Application source code for the social media app. |
| **role.yml** | Kubernetes Role defining permissions for Jenkins. |
| **rolebinding.yml** | Binds the Role to the `jenkins` ServiceAccount. |
| **serviceaccount.yml** | Creates the Jenkins ServiceAccount in the `webapps` namespace. |
| **secret.yml** | Generates the token Secret linked to the Jenkins ServiceAccount. |
| **README.md** | Project documentation (this file). |

---

## ⚙️ Tools & Technologies Used

- **AWS EKS** – Managed Kubernetes cluster
- **Terraform** – Infrastructure as Code for provisioning EKS and networking
- **Jenkins** – CI/CD automation
- **Kubernetes (kubectl)** – Deployment management
- **Docker** – Containerization
- **GitHub** – Source code repository

---

## 🚀 Deployment Flow

1. **Provision EKS Cluster**
   ```bash
   terraform init
   terraform apply -auto-approve
