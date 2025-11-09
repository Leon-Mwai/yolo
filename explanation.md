# Project Explanation

## 1. Choice of Kubernetes Objects

For this project, I used **Deployments** for the frontend and backend applications to ensure scalability and self-healing. For the database, I used a **StatefulSet** because it requires stable network identity and persistent storage. StatefulSets guarantee that pods are started, stopped, and rescheduled in order, preserving the stateful nature of the database.

## 2. Method Used to Expose Pods to Internet Traffic

To expose the applications, I used **LoadBalancer** Services. This allows external traffic to reach the pods via an external IP provided by GKE. Internal communication between the backend and database uses **ClusterIP** services for secure and efficient connectivity.

## 3. Use of Persistent Storage

Persistent Volumes (PV) and Persistent Volume Claims (PVC) were implemented for the database StatefulSet. This ensures that even if the database pod is deleted or rescheduled, the data persists and is not lost, fulfilling the requirement for durable storage.

## 4. Git Workflow

The repository follows a structured Git workflow with over 10 descriptive commits tracking the project evolution from setup to deployment. This aids transparency and maintainability, allowing reviewers to follow the project's progression clearly.

## 5. Docker Image Tag Naming

Docker images are tagged under my Docker Hub username `leonmwai` with clear tags like `backend:latest` and `client:latest` to maintain clarity and version control.

