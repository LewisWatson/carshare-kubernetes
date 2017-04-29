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

## Deploy Ingress Controller

Ingress is not enabled by default in [minikube].

```bash
minikube addons enable ingress
```

Initialise helm on the cluster

```bash
helm init
```

Deploy the [NGINX Ingress Helm Chart][nginx-ingress].

```bash
helm install --name my-release stable/nginx-ingress
```

[minikube]: https://github.com/kubernetes/minikube
[helm]: https://github.com/kubernetes/helm
[nginx-ingress]: https://github.com/kubernetes/charts/tree/master/stable/nginx-ingress