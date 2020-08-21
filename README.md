# ATF Proof of concept - Containerize Node.js with docker and run on Kubernetes or OpenShift cluster

## Prerequisites
You must install and configure the following tools before moving forward
* node.js
* docker
* minikube (for Kuberntes cluster only)
* kubectl(Kuberntes cluster) or oc(OpenShift cluster)
* helm
* Kubernetes or OpenShift cluster must be running

## Usage

### Docker

Use docker to buid & push to docker hub

```bash
docker build -t <your username>/<appname> .

docker login                           

docker push  <your username>/<appname> 
```

Replace the repository in values.yaml with your docker image

### Running on Kubernetes cluster

Use helm to package & deploy to Kubernetes cluster 

```bash
helm package atfnodejsdocker

helm install atfnodejsdocker ./atfnodejsdocker-0.1.0.tgz
```

Verify the Pod & service in Kubernetes cluster 

```bash
kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
atfnodejsdocker-5b5fc7dd65-n7h4g   1/1     Running   0          69m
```
Get the url of the application in Kubernetes cluster

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

Test via the following url

```bash
curl http://127.0.0.1:59798
Hello world
```

### Running on OpenShift cluster

Create project in OpenShift cluster 

```bash
oc new-project atfnodejsdocker
```

Use helm to package & deploy to OpenShift cluster 

```bash
helm package atfnodejsdocker

helm install atfnodejsdocker ./atfnodejsdocker-0.1.0.tgz
```

Verify the Pod & service in OpenShift cluster 

```bash
oc get pods
NAME                               READY   STATUS    RESTARTS   AGE
atfnodejsdocker-5b5fc7dd65-vm9gv   1/1     Running   0          11m

oc get svc
NAME              TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
atfnodejsdocker   NodePort   172.25.194.218   <none>        3000:30000/TCP   12m
```

Expose the  service in OpenShift cluster 

```bash
oc expose svc/atfnodejsdocker
route.route.openshift.io/atfnodejsdocker exposed
```

Get the url of the application in OpenShift cluster 

```bash
oc get route
NAME              HOST/PORT                                          PATH   SERVICES          PORT   TERMINATION   WILDCARD
atfnodejsdocker   atfnodejsdocker-atfnodejsdocker.apps-crc.testing          atfnodejsdocker   http                 None
```

Test via the url generated from HOST/PORT from the previous command

```bash
curl http://atfnodejsdocker-atfnodejsdocker.apps-crc.testing
Hello world
```
