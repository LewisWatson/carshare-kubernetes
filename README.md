# carshare-kubernetes

## Prerequisites

* [minikube] v0.14.0+
* [Kubernetes Helm][helm]

## Start [minikube]

```bash
minikube start
```

## Deploy [Mongo Data Store][mongo]

[carshare-api] uses [mongo] for data persistence.

```bash
kubectl create -f mongo-rc.yaml
kubectl create -f mongo-service.yaml
```

## Deploy [Carshare API][carshare-api]

```bash
kubectl create -f carshare-api-rc.yaml
kubectl create -f carshare-api-service.yaml
```

## Deploy [Carshare Web][carshare-web]

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

You can test the system by adding `carshare.test` to your `/ets/hosts` file.

```bash
echo "$(minikube ip) carshare.test" | sudo tee -a /etc/hosts
```

You should be able to access [carshare-web] by navigating to [carshare.test](http://carshare.test) in a browser.

![carshare-web screenshot](/docs/img/carshare-web.png)

You should also be able to make a `GET` request on [carshare-api].

```bash
$ curl http://carshare.test/api/v0/carShares
{"errors":[{"status":"403","title":"Forbidden"}]}
```

 It should respond with an error 404 forbidden as you will not be authenticated.


[minikube]: https://github.com/kubernetes/minikube
[helm]: https://github.com/kubernetes/helm
[nginx-ingress]: https://github.com/kubernetes/charts/tree/master/stable/nginx-ingress
[carshare-web]: https://github.com/LewisWatson/carshare-web
[carshare-api]: https://github.com/LewisWatson/carshare-back
[mongo]: https://hub.docker.com/_/mongo/