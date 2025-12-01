
Installing Argo

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

#get the password 
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

#port forward 
kubectl port-forward svc/argocd-server -n argocd 8080:443

#URL: https://localhost:8080
```

