docker rate limits
Log in to hub.docker.com.
Go to Account Settings > Security.
Click New Access Token.
Give it a description (e.g., "k3s-homelab") and copy the token.



You create the secret once in a central namespace (like kube-system).
You add a special "annotation" to it.
Reflector watches for that annotation and automatically copies the secret to all other namespaces.

Step 1: Install Reflector
Step 2: Generate the Master Secret YAML
Step 3: Add Reflector Annotations

```
kubectl create secret docker-registry regcred \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=<YOUR_DOCKERHUB_USERNAME> \
  --docker-password=<YOUR_ACCESS_TOKEN> \
  --docker-email=<YOUR_EMAIL> \
  --namespace kube-system \
  --dry-run=client -o yaml > regcred.yaml
```

step 4

```
apiVersion: v1
kind: Secret
metadata:
  name: regcred
  namespace: kube-system
  # --- ADD THESE LINES ---
  annotations:
    reflector.v1.k8s.emberstack.com/reflection-allowed: "true"
    reflector.v1.k8s.emberstack.com/reflection-allowed-namespaces: "*"
    reflector.v1.k8s.emberstack.com/reflection-auto-enabled: "true"
  # -----------------------
  creationTimestamp: null
type: kubernetes.io/dockerconfigjson
data:
  .dockerconfigjson: <YOUR_ENCRYPTED_DATA_HERE>
```

step 5
```
kubectl apply -f regcred.yaml
```

step 6 > sync reflector