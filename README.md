# Three-Tier Application Project - EFE Egypt DevOps Training

This project was my first hands-on experience as part of the **EFE Egypt DevOps Training Program**. It involved building and deploying a three-tier application, providing practical exposure to containerization, Kubernetes, and DevOps workflows.  

## Project Overview

The application consists of a **Go backend** connected to a **MySQL database**. The project workflow included two main stages:

### Local Setup with Docker Compose
- Run the backend and database as separate containers.  
- Ensure proper communication between the backend and database using Docker networking.  
- Manage environment variables for both backend and database using `.env` files.  

### Kubernetes Setup with Minikube
- Created **Deployments** and **Services** for both backend and database.  
- Managed environment variables using **ConfigMaps**.  
- Stored sensitive information like database passwords securely using **Secrets**.  
- Set up **Persistent Volumes** for MySQL data to maintain data across pod restarts.  

This helped me understand how to translate a simple containerized application into a **Kubernetes-based architecture**, while managing configuration and data securely in a cluster environment.  

## Learning Outcomes
- Practical knowledge of **Docker Compose** and container orchestration.  
- Experience with **Kubernetes Deployments, Services, ConfigMaps, Secrets, and Persistent Volumes**.  
- Understanding how to structure and manage a cloud-native, scalable application.  
- Exposure to real-world **DevOps practices** for deploying and managing applications.  

## Acknowledgements
I would like to thank **Gamal Mohammad**, **Abdelrahman Ahmed**, Senior Academy - Professional Training, and the **EFE Egypt team** for guiding us through the training program and providing practical, hands-on learning.

---

**Author:** Bassant Mohy  
DevOps Enthusiast | CI/CD | Kubernetes | Docker | Cloud-Native Applications
