# get eks kubeconfig

```bash
aws eks update-kubeconfig --name oopscraft
```

# check kubeconfig context

```bash
kubectl config get-contexts 
```

# checks current context 

```bash
kubectl config current-context
```

# switch context

```bash
kubectl config use-context minikube 
```

# call specific context

```bash
kubectl get all --context minikube

```
