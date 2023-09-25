# Minikube SDLC tools
Configuration of SDLC tools on minikube

## Install minikube

Instructions for installing minikube: [minikube start](https://minikube.sigs.k8s.io/docs/start/)

**Note:** Make sure to configure sufficient disk-space for minikube to cater for persistent volumes
```
minikube config set disk-size 100G
```

## Install Windows tools
- Install Chocolatey: https://www.studytonight.com/post/install-chocolatey-package-manager-for-windows
- Install Kubectl: 
- Install Helm: https://www.studytonight.com/post/installing-kubernetes-helm-on-windows

## Create sdlc-tools namespace
```
kubectl create -f .\kubernetes\namespaces\sdlc-tools.yaml
```

## Deploy Jenkins
### Create Peristent Volume
```
kubectl create -f .\kubernetes\volumes\sdlc-jenkins-pv.yaml
```

**Note:** You will need to manually configure the permissions on the volume once it has been created
```
minikube ssh
sudo chown -R 1000:1000 /data/jenkins-volume
```


### Create Service Account
```
kubectl create -f .\kubernetes\authorization\sdlc-jenkins-sa.yaml
```

### Deploy Helm chart
```
helm repo add jenkinsci https://charts.jenkins.io
helm repo update 
helm install jenkins -n sdlc-tools -f .\kubernetes\helm-charts\sdlc-jenkins-values.yml jenkinsci/jenkins
```

### Get admin password (Powershell)
```
$jsonpath="{.data.jenkins-admin-password}"
$secret=$(kubectl get secret -n sdlc-tools jenkins -o jsonpath=$jsonpath)
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($secret))
```

### Get the Jenkins URL
```
$jsonpath="{.spec.ports[0].nodePort}"
$NODE_PORT=$(kubectl get -n sdlc-tools -o jsonpath=$jsonpath services jenkins)
$jsonpath="{.items[0].status.addresses[0].address}"
$NODE_IP=$(kubectl get nodes -n sdlc-tools -o jsonpath=$jsonpath)
echo "http://${NODE_IP}:${NODE_PORT}/login"
```

