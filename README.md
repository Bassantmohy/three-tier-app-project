# Project Deployment on OpenShift (CRC)

This project demonstrates deploying my application on **Minikube** and **OpenShift (CRC)**, as part of the **DevOps Engineer Program** during the DevOps training. Throughout the deployment process, I faced several challenges that provided valuable learning experiences.

## Challenges and Solutions

- **Permission issue for `/var/lib/mysql`**  
  Even though the service account `anyuid` was assigned, the pod couldn’t access the volume.  
  **Solution:** Set a specific user ID to match what the Persistent Volume Claim (PVC) expects.

- **Backend password path issue**  
  The backend couldn’t locate the database password.  
  **Solution:** Provided the password using `path-subpath` in the volume configuration.

- **Nginx port permissions**  
  Initially, Nginx couldn’t access ports 443 and 80.  
  **Solution:** Adjusted the ports to 8080 and 8443.

- **Other adjustments**  
  Made several smaller changes, like modifying the StorageClass and other configurations to ensure smooth deployment.

## Lessons Learned

Deploying on different environments like Minikube and OpenShift requires careful attention to **permissions, paths, and network configurations**. Each challenge became an opportunity to learn and improve my deployment skills.

## Repository

[Project Repository Link](https://lnkd.in/dJn6MHHx)

## Acknowledgements

Many thanks to **Gamal Mohammad**, **Abdelrahman Ahmed**, Senior Academy - Professional Training for their support and guidance during the training.

---

**Author:** Bassant Mohy  
DevOps Enthusiast | CI/CD | Kubernetes | OpenShift | Minikube
