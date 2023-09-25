# Minikube SDLC tools
Configuration of SDLC tools on minikube

## Install minikube

Instructions for installing minikube: [minikube start](https://minikube.sigs.k8s.io/docs/start/)

## Install kubectl

## Create namespaces
```
kubectl create -f .\kubernetes\namespaces\sdlc-ansible-namespace.yaml
kubectl create -f .\kubernetes\namespaces\sdlc-jenkins-namespace.yaml
```

