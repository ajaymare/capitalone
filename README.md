# capitalone
capital One Demo

# TSB Access
```sh
https://capitalone.azure.sandbox.tetrate.io/
username: admin
password: Tetrate123
```

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

## Access tier1 gateway
```sh
while true                                      
do
kubectl exec deploy/sleep -n sleep -- curl -sk -I http://bookinfo.tetrate.io/productpage --resolve "bookinfo.tetrate.io:443:135.233.124.38"
sleep 1
done
```
## Gateway Failover on CenterUS
```sh
kubectl scale deployment bookinfo-ugw -n bookinfo --replicas=0
```
## Bring Gateway Back
```sh
kubectl scale deployment bookinfo-ugw -n bookinfo --replicas=1 
```

## Eastwest Traffic Failover to useast cluster
## Access Application internally 
```sh
while true                                      
do
kubectl exec deployment/sleep -n sleep -c sleep -- curl -s -H "X-B3-Sampled: 1" http://productpage.bookinfo:9080/productpage | grep -i details -A 8
sleep 1
done
```
## Simulate Deployment Failure by scaling down details pod on CentralUS
```sh
kubectl scale deployment details-v1 -n bookinfo --replicas=0
```
## Traffic should shift to UsEast Topology

# Ambient Mode

## waypoint is automatically instantiated per namespace where the ambient deployment exists
```sh
kubectl get gateway waypoint -n caffeine -o yaml
```
## Validate ztunnel configuration
```sh
istioctl ztunnel-config  service -n istio-system | grep caffeine
istioctl ztunnel-config workload  -n cnp-istio | grep caffeine
```

