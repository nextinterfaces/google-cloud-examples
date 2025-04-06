# Deploy Nginx Helm to GCloud


## ✅ Prerequisites

    brew install kubectl
    brew install helm
    brew install google-cloud-sdk # see gcloud-commands.md

    kubectl version --client
    helm version
    gcloud version

### Authenticate to Google Cloud

    gcloud auth login
    gcloud config set project YOUR_PROJECT_ID
    gcloud auth application-default login

### Enable Required APIs

    gcloud services enable container.googleapis.com

## 🚀 Step-by-Step Deployment to GKE

### Create a GKE Cluster (if not created with dashboard)

    gcloud container clusters create my-cluster-1 --zone us-central1-a --num-nodes 2    

### ✅ Step 1: Connect kubectl to GKE Autopilot Cluster
    
    gcloud container clusters get-credentials autopilot-cluster-1 --region us-central1

    -> kubeconfig entry generated for autopilot-cluster-1.

Check access

    kubectl get nodes
    kubectl get ns
    kubectl get pods -A

### ✅ Step 2: Add the Bitnami Helm Repo

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm repo update

### ✅ Step 3: Create Namespace (optional but clean)

    kubectl create namespace web-test

### ✅ Step 4: Install NGINX Chart into Autopilot GKE

    helm install my-nginx bitnami/nginx \
    --namespace web-test \
    --set service.type=LoadBalancer \
    --set containerSecurityContext.runAsNonRoot=true \
    --set containerSecurityContext.allowPrivilegeEscalation=false \
    --set "containerSecurityContext.capabilities.drop[0]=ALL"

### ✅ Step 5: Verify and Access

    kubectl get svc -n web --watch

    kubectl get svc --namespace web-test -w my-nginx

    -> NAME       TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
    -> my-nginx   LoadBalancer   34.118.236.151   34.135.118.232   80:30271/TCP,443:31089/TCP   2m11s

### ✅ Step 7: Cleanup (Optional)

    helm uninstall my-nginx -n web-test
    kubectl delete namespace web-test
