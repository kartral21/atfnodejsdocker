# ATF Proof of concept - Containerize Node.js with docker and run on kubernetes cluster

## Prerequisites
You must install and configure the following tools before moving forward
* node.js
* docker
* minikube
* kubectl
* helm
* kubernetes cluster must be running

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
NAME                               READY   STATUS    RESTARTS   AGE
atfnodejsdocker-5b5fc7dd65-n7h4g   1/1     Running   0          69m
atfrobotdocker-6cc6cd4db4-72x6x    1/1     Running   0          89m
```
To test via browser, get url of the application using below commands

```bash
minikube service list
|-------------|-----------------|--------------|-----|
|  NAMESPACE  |      NAME       | TARGET PORT  | URL |
|-------------|-----------------|--------------|-----|
| default     | atfnodejsdocker | http/3000    |     |
| default     | atfrobotdocker  | No node port |
| default     | kubernetes      | No node port |
| kube-system | kube-dns        | No node port |
|-------------|-----------------|--------------|-----|


minikube service atfnodejsdocker --url
🏃  Starting tunnel for service atfnodejsdocker.
|-----------|-----------------|-------------|------------------------|
| NAMESPACE |      NAME       | TARGET PORT |          URL           |
|-----------|-----------------|-------------|------------------------|
| default   | atfnodejsdocker |             | http://127.0.0.1:59798 |
|-----------|-----------------|-------------|------------------------|
http://127.0.0.1:59798
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.
```

Test via the following url:
```bash
curl http://127.0.0.1:59798
Hello world
```
