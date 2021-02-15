# MiniKube
```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.0/bin/windows/amd64/kubectl.exe

$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
$ sudo dpkg -i minikube_latest_amd64.deb

# start
$ sudo minikube start --driver=none
```

# Kubernetes

## Installation(Docker)
```
// remove previous version
$ sudo apt-get remove docker docker-engine docker.io

// install library
$ sudo apt-get install apt-transport-https ca-certificates curl

// install valid version
$ sudo apt-get install docker-ce=18.06.2~ce~3-0~ubuntu

// test docker run
$ sudo docker run hello-world

// Set up the Docker daemon
$ cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

// Create /etc/systemd/system/docker.service.d
$ sudo mkdir -p /etc/systemd/system/docker.service.d

// Restart Docker
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker

// starts on reboot
$ sudo systemctl enable docker

```

## Installation(Kubernetes)
```
// updates repository
$ sudo apt-get update && sudo apt-get install -y apt-transport-https curl
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
$ sudo apt-get update

// installs kube series
$ sudo apt-get install -y kubelet kubeadm kubectl

// marks hold
$ sudo apt-mark hold kubelet kubeadm kubectl

// disable swap 
$ sudo swapoff -a 

```

## Startup and Shutdown
```
// checks ip
$ sudo ifconfig

// kubeadm init and copy join cluster command
$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.0.2

// stop(reset)
$ sudo kubeadm reset

// cp files
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

// install pod network add-on
$ sudo kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml
$ sudo kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml

// test kubectl
$ sudo kubectl get nodes
$ sudo kubectl get pods
$ sudo kubectl get events


```

## Join Cluster
```
// join
$ kubeadm join 192.168.0.2:6443 --token ppku3i.nc1ajtz3du47f7yq \
    --discovery-token-ca-cert-hash sha256:5b990c5abc0eb6b454f14b025a3f27085a071dffdc0e7479064e2e0d609ce479
```

## Command
```
// view cluster info
$ sudo kubectl cluster-info

// view version
$ sudo kubectl version --short

// list nodes
$ sudo kubectl get nodes

// list pods
$ sudo kubectl get pods


```

## cronJob
```

```
