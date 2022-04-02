# ArgoCD and ArgoCD Rollouts

### Install ArgoCD
kubectl create namespace argocd
kubectl apply -f argocd/install.yaml -n argocd

### Install ArgoCD Rollouts
kubectl create namespace argo-rollouts
kubectl apply -f argo-rollouts/install.yaml -n argo-rollouts
