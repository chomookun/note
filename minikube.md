# Minikube

## Insecure registry

```selll
vim ~/.minikube/machines/minikube/config.json
...
"InsecureRegistry": [
  ...
  "192.168.0.2/12"
]
...
```

