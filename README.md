# ATF POC - Containerize NodeJS with Docker and run on Kubernetes

## Prerequisites
You must install and configure the following tools before moving forward
* nodeJS
* nocker
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
kubectl get pods 
```
To test via browser, get url of the application using below commands:

```bash
minikube service list // To get service name
minkube service <service-name> --url // To get access url for your application
```
