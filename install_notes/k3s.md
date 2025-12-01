Installing K3s

```
#k8s box
curl -sfL https://get.k3s.io | sh -s - --disable traefik

#kubectl on k8s box
sudo cp /etc/rancher/k3s/k3s.yaml ~/k3s-config
sudo chown $USER:$USER ~/k3s-config

# client box 
scp <user>>@<192.168.1.X>:~/k3s-config ./config

code ./config

#modify / update the ip to the server ip 
# move it to your kube config folder location > connect 
```