
## Create service account and rbac
```
kubectl apply -f k8s-admin-sa.yml
kubectl apply -f k8s-admin-rbac.yml
```

## Get token or generate token
For <= 1.23
```
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep k8s-admin | awk '{print $1}')
```

For 1.24+
Download and install the latest kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

```
kubectl create token k8s-admin --duration=999999h -n kube-system
```

## Get certificate-authority-data and server info 
```
kubectl config view --flatten --minify > kubeconfig
cat kubeconfig
```

## Generate kubeconfig, replacetoken&certificate-authority-data&server as above
```
apiVersion: v1
kind: Config
users:
- name: k8s-admin
  user:
    token: <replace this with token info>
clusters:
- cluster:
    certificate-authority-data: <replace this with certificate-authority-data info>
    server: <replace this with server info>
  name: mycluster
contexts:
- context:
    cluster: mycluster
    user: k8s-admin
  name: k8s-admin@mycluster
current-context: k8s-admin@mycluster
```


