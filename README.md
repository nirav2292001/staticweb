#  🚀 GitOps Pipeline with GitHub Actions & ArgoCD 

🌟 Overview
This project demonstrates a CI/CD pipeline using GitHub Actions, Docker, Kubernetes, and ArgoCD. It automates application deployment to a Kubernetes cluster using a GitOps approach.

🎯 Key Features
✅ GitHub Actions for CI/CD automation
✅ Docker Hub for container image storage
✅ Kubernetes for container orchestration
✅ ArgoCD for GitOps-based deployment
✅ Auto-syncing Kubernetes manifests with GitHub commits

🛠 Tech Stack
🐳 Docker
☸️ Kubernetes
🤖 GitHub Actions
🔹 ArgoCD
📦 Docker Hub


🚀 Setup & Installation

🔹 1️⃣ Prerequisites
Ensure you have the following installed:
Docker
Kubernetes (kubectl)
ArgoCD
GitHub Repository with required secrets (DOCKER_USERNAME, DOCKER_PASSWORD)

🔹 2️⃣ Clone the Repository
git clone https://github.com/nirav2292001/staticweb
cd staticweb

🔥 How to Configure ArgoCD with Your GitHub Repository
ArgoCD follows the GitOps approach, meaning it syncs your Kubernetes manifests from a Git repository. Follow these steps to configure ArgoCD with your repo and ensure automatic deployments.

📌 Step 1: Install & Expose ArgoCD

✅ 1️⃣ Install ArgoCD
Run the following commands to install ArgoCD in your Kubernetes cluster:
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

✅ 2️⃣ Expose ArgoCD Server (For UI Access)
By default, ArgoCD runs as a ClusterIP service (not accessible externally). Change it to a NodePort or LoadBalancer:
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
kubectl port-forward svc/argocd-server -n argocd 8080:443

📌 Step 2: Log in to ArgoCD
✅ 1️⃣ Get the Initial Admin Password
The default admin password is stored as a Kubernetes secret. Get it with:
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode

📌 Step 3: Create an ArgoCD Application
Now, tell ArgoCD which Kubernetes manifests it should track.

✅ 1️⃣ Create an Application via UI
Go to "Applications" → "New Application"
Fill in the details:
Application Name: static-web
Project: default
Sync Policy: Automatic (Recommended) or Manual
Repository URL: https://github.com/yourusername/yourrepo.git
Path: k8s-manifests/ (Folder where your manifests are stored)
Cluster URL: https://kubernetes.default.svc
Namespace: default
Click "Create".

📌 Step 5: Sync & Deploy the Application
If you enabled Automatic Sync, ArgoCD will deploy automatically.
Otherwise, manually sync it:
✅ 1️⃣ Sync via UI
Go to "Applications" → Click "static-web" → Click "Sync".

⚡ CI/CD Workflow
1️⃣ Code Push to GitHub → GitHub Actions triggers CI/CD
2️⃣ Build & Push Docker Image → Docker Hub
3️⃣ Update Kubernetes Deployment Manifest
4️⃣ ArgoCD Syncs Changes Automatically

📌 Step 6: Verify Deployment
kubectl get pods
kubectl get svc

If using a ClusterIP service, forward the port:
kubectl port-forward svc/static-web 9090:80

Then, open http://localhost:9090 in your browser.





