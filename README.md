```
kubectl apply -f k8s-admin-sa.yml
kubectl apply -f k8s-admin-rbac.yml
```

```
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep k8s-admin | awk '{print $1}')
```

For 1.24+, or refer https://www.programmingwithwolfgang.com/use-the-tokenrequest-api-to-create-token-in-kubernetes/
```
kubectl create token k8s-admin -n kube-system
```

```
kubectl config view --flatten --minify > kubeconfig
cat kubeconfig
```

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


