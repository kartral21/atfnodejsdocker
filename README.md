# ATF POC - Containerize Node.js with docker and run on kubernetes cluster

## Prerequisites
You must install and configure the following tools before moving forward
* node.js
* docker
* minikube
* kubectl
* helm
* Kubernetes cluster must be running

## Usage

### Docker

Use docker to buid & push to docker hub

```bash
docker build -t <your username>/<appname> .

docker login                           // To login into dockerhub

docker push  <your username>/<appname> // push image to your repo
```

### Helm

Use helm to package & deploy to Kubernetes cluster 

```bash
helm package atfnodejsdocker // package the helm chart

helm install atfnodejsdocker ./atfnodejsdocker-0.1.0.tgz
```
### Kubernetes

Verify the Pod & service in Kubernetes cluster 

```bash
karthiks-mbp:atfnodejsdocker karthikrallapalli$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
atfnodejsdocker-5b5fc7dd65-n7h4g   1/1     Running   0          69m
atfrobotdocker-6cc6cd4db4-72x6x    1/1     Running   0          89m
```
To test via browser, get url of the application using below commands:

```bash
minikube service list // To get service name
minkube service <service-name> --url // To get access url for your application
```
