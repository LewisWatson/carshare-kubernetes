# carshare-kubernetes

## Prerequisites

* [minikube] v0.14.0+
* [Kubernetes Helm][helm]

## Start [minikube]

```bash
minikube start
```

## Deploy MongoDB Data Store

```bash
kubectl create -f mongo-rc.yaml
kubectl create -f mongo-service.yaml
```

## Deploy Carshare API

```bash
kubectl create -f carshare-api-rc.yaml
kubectl create -f carshare-api-service.yaml
```

## Deploy Carshare Web

```bash
kubectl create -f carshare-web-rc.yaml
kubectl create -f carshare-web-service.yaml
```

## Configure Ingress

Ingress is not enabled by default in [minikube].

```bash
minikube addons enable ingress
```

Initialise [helm] on the cluster

```bash
helm init
```

Deploy the [NGINX Ingress Helm Chart][nginx-ingress].

```bash
helm install --name my-release stable/nginx-ingress
```

Create Ingress

```bash
kubectl create -f ingress.yaml
```

## Test

You can test the system by adding `carshare.test` to your `/ets/hosts` file and performing a `GET` request on the API. It should repond with an error 404 forbidden as you will not be authenticated.

```bash
$ echo "$(minikube ip) carshare.test" | sudo tee -a /etc/hosts
$ curl http://carshare.test/api/v0/carShares
{"errors":[{"status":"403","title":"Forbidden"}]}
```

[minikube]: https://github.com/kubernetes/minikube
[helm]: https://github.com/kubernetes/helm
[nginx-ingress]: https://github.com/kubernetes/charts/tree/master/stable/nginx-ingress