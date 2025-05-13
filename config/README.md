# Secrets Management

This directory contains templates for Kubernetes secrets. DO NOT store actual secrets in Git.

## Setting up secrets in ArgoCD:

1. Create secrets in your cluster using:
   ```bash
   kubectl create secret generic minio-credentials \
     --from-literal=MINIO_ACCESS_KEY='your-access-key' \
     --from-literal=MINIO_SECRET_KEY='your-secret-key' \
     --namespace microservices

   kubectl create secret generic mongodb-credentials \
     --from-literal=MONGODB_USER='your-mongodb-user' \
     --from-literal=MONGODB_PASSWORD='your-mongodb-password' \
     --namespace microservices

   kubectl create secret generic rabbitmq-credentials \
     --from-literal=RABBITMQ_USER='your-rabbitmq-user' \
     --from-literal=RABBITMQ_PASSWORD='your-rabbitmq-password' \
     --namespace microservices
   ```

2. Configure ArgoCD to not manage these secrets (add to your Application manifest):
   ```yaml
   spec:
     ignoreDifferences:
     - group: ""
       kind: Secret
       jsonPointers:
       - /data
   ```