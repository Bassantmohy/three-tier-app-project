# Jenkins CI/CD Pipeline Project

This project demonstrates a complete **CI/CD pipeline** using **Jenkins, Helm, Kubernetes, and Docker**, built as part of a **DevOps training**.

---

## Project Overview

In this project, Jenkins is deployed as a pod on Kubernetes using **Helm**. Custom YAML files are packaged as a **Helm chart** and pushed to GitHub. The pipeline automates building, deploying, testing, and notifying about the backend application.

---

## Pipeline Stages

1. **Build Stage**  
   - Builds the backend Docker image.  
   - Pushes the image to **Docker Hub**.

2. **Deploy Stage**  
   - Updates YAML files dynamically using **Helm**.  
   - Deploys the application to Kubernetes.

3. **Dynamic Agent Stage**  
   - Creates Jenkins agents dynamically when a job starts.  
   - Terminates agents automatically after the job completes to save resources.

4. **Smoke Test Stage**  
   - Runs a simple test to verify the **proxy service is healthy**.

5. **Notification Stage**  
   - Sends **email notifications** with the results of the pipeline.

---

## Features

- Full CI/CD automation.
- Efficient resource usage with dynamic agents.
- Smoke testing for service health.
- Email notifications for pipeline results.
- Version-controlled Helm charts and `values.yaml`.

---

## Technologies Used

- **Jenkins**  
- **Helm**  
- **Kubernetes**  
- **Docker**  
- **GitHub**

  
![Screenshot](Screenshot%20from%202025-11-20%2018-39-10.png)
