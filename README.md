+++markdown
# MongoDB Deployment on Kubernetes with ArgoCD

This repository contains YAML manifests to deploy a MongoDB instance on a Kubernetes cluster. The deployment process is automated using **ArgoCD**, which monitors the Git repository and applies the configuration to the cluster.

---

## Project Structure

The repository includes the following Kubernetes manifests:

- **Namespace Configuration**: Creates a dedicated namespace for MongoDB.
- **Deployment**: Defines the MongoDB pod and its persistent volume configuration.
- **Service**: Exposes MongoDB for external or internal access.
- **PersistentVolumeClaim (PVC)**: Requests storage for MongoDB data.

---

## Deployment Workflow with ArgoCD

### Overview

1. **Push Manifests to Git**: All Kubernetes manifests are stored in this Git repository.
2. **ArgoCD Sync**: ArgoCD continuously monitors the repository for changes.
3. **Automated Deployment**: When changes are detected, ArgoCD automatically deploys or updates the MongoDB resources on the Kubernetes cluster.

---

## Steps to Deploy with ArgoCD

### Prerequisites

1. A running Kubernetes cluster with `kubectl` configured.
2. `nfs-client` storage class available in the cluster.
3. ArgoCD installed and accessible.

### Deployment Process

1. **Push Files to GitHub**:  
   Ensure all the YAML manifests are committed and pushed to a Git repository.

2. **Create an ArgoCD Application**:  
   - Open the ArgoCD UI.
   - Click **New Application**.
   - Fill in the application details:
     - **Project**: Default or a custom project.
     - **Repository URL**: URL of your Git repository.
     - **Path**: Folder containing the YAML files (if applicable).
     - **Namespace**: `mongo-namespace`.
     - **Sync Policy**: Manual or Automatic (for continuous updates).

3. **Sync Application**:  
   Once the application is created, click **Sync** in the ArgoCD UI. ArgoCD will deploy the MongoDB resources to your cluster.

4. **Monitor Deployment**:  
   - Use the ArgoCD dashboard to monitor the state of the application.
   - Ensure all resources are in a **Healthy** state.

---

## Verifying the Deployment

1. **Check Namespace**:
   ```bash
   kubectl get namespace
   ```

2. **List Pods**:
   ```bash
   kubectl get pods -n mongo-namespace
   ```

3. **Verify Service**:
   ```bash
   kubectl get svc -n mongo-namespace
   ```

4. **Check PVC**:
   ```bash
   kubectl get pvc -n mongo-namespace
   ```

---

## Updating the Deployment

To update the deployment, modify the YAML manifests in the Git repository and push the changes. ArgoCD will detect the updates and automatically sync the resources, depending on the sync policy configured.

---

## Cleanup

To delete the deployment:

1. **From ArgoCD**:  
   - Delete the application from the ArgoCD UI.
   - ArgoCD will clean up all associated resources.

2. **Manually via Kubernetes**:
   ```bash
   kubectl delete namespace mongo-namespace
   ```

---

## Notes

- Ensure the `nfs-client` storage class is correctly configured for PVCs.
- Monitor ArgoCD logs to troubleshoot any issues during deployment.

---
