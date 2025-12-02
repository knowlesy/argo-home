
Installing Argo


```
kubectl create namespace argocd
kubectl label namespace argocd istio-injection=enabled

# Add port 5400 to Istio ingress gateway for ArgoCD
kubectl patch svc istio-ingressgateway -n istio-system --type='json' \
  -p='[{"op": "add", "path": "/spec/ports/-", "value": {"name": "http-argocd", "port": 5400, "protocol": "TCP", "targetPort": 5400}}]'
```

Install ArgoCD via Helm

```

helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

helm install argocd argo/argo-cd -n argocd \
  -f apps-values/argocd/values.yaml \
  --version 6.7.0

kubectl apply -f bootstrap/root-app.yaml
```


 Get the password 

```

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo


```



In your private repo create the following path 

`configs/pihole/values.yaml` 

Populate the following 

```
# Run this in your terminal, or save as a file and apply it
apiVersion: v1
kind: Secret
metadata:
  name: private-repo-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/<YOUR_GITHUB_USERNAME>/argo-private.git
  username: <YOUR_GITHUB_USERNAME>
  password: <YOUR_GITHUB_PAT>
  ```
The perms youll need (not using classic method)
`Contents`: Read-only (This allows reading the files/YAML).
`Metadata`: Read-only (This is usually selected by default and allows reading repo stats).


  then execute it locally 
  `kubectl apply -f <filename>.yaml`