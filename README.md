# capitalone
capital One Demo

# Deploying ArgoCD
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
## Port Forwad to access ArgoCD
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
## ArgoCD Password
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

# Deploying Application on Clusters using GitOps
Argo will deploy the applications on clusters and configure TSB


