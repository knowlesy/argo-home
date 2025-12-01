
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

  then execute it locally 
  `kubectl apply -f <filename>.yaml`