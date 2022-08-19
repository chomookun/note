# installation

```bash
# create kube namespace
kubectl create namespace argocd

# install
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# checks
kubectl get all -n argocd

```

# gets admin password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

```

# create port-forward to argocd-server

```bash
kubectl port-forward --address 0.0.0.0 -n argocd service/argocd-server ${port}:80

```



