
Installing Argo


```

helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl patch configmap argocd-cmd-params-cm -n argocd --type merge -p '{"data":{"server.insecure":"true"}}'
```

Configure 
```
bootstrap/example-argo-ingress.yaml
```

then execute
```
kubectl apply -f argo-ingress.yaml
```

```
kubectl apply -f bootstrap/root-app.yaml
```


 Get the password 

```

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo


```



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