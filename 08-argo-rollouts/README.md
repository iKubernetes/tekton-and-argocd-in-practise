# Argo Rollouts Examples.

### 01-argo-rollouts-demo.yaml

```bash
kubectl create namespace demo
kubectl apply -f 01-argo-rollouts-demo.yaml -n demo
```

#### rollout

```bash
kubectl argo rollouts set image rollouts-spring-boot-helloworld spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.6 -n demo
```

#### watch

```bash
kubectl argo rollouts get rollout rollouts-spring-boot-helloworld --watch -n demo
```

#### Promote

```bash
kubectl argo rollouts promote rollouts-spring-boot-helloworld -n demo
```

### 02-argo-rollouts-with-istio-traffic-shifting.yaml

**Dependicies**: Istio

```bash
kubectl create namespace demo
kubectl label namespace demo istio-injection=enabled
kubectl apply -f 02-argo-rollouts-with-istio-traffic-shifting.yaml -n demo
```

#### Client
Install a client, and send request to http://spring-boot-helloworld.demo.svc

```bash
kubectl run client-$RANDOM --image=ikubernetes/admin-box:v1.2 --rm -it --restart=Never --command -- /bin/bash
```

```bash
while true; do curl http://spring-boot-helloworld.demo.svc.cluster.local; echo; sleep 1; done
```

#### Rollout

```bash
kubectl argo rollouts set image rollouts-helloworld-with-traffic-shifting spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.6 -n demo
```

### 03-argo-rollouts-with-analysis.yaml

**Dependicies**: Istio and Prometheus

```bash
kubectl create namespace demo
kubectl label namespace demo istio-injection=enabled
kubectl apply -f 03-argo-rollouts-with-analysis.yaml -n demo
```

#### Prometheus Metrics

```
irate of requests that the response code not 5xx: sum(irate(istio_requests_total{reporter="source",destination_service="spring-boot-helloworld.default.svc.cluster.local",response_code!~"5.*"}[1m]))
```

```
irate of all requests: irate(istio_requests_total{reporter="source",destination_service="spring-boot-helloworld.default.svc.cluster.local"}[1m])
```

Successful Rate:

```
sum(irate(istio_requests_total{reporter="source",destination_service="spring-boot-helloworld.default.svc.cluster.local",response_code!~"5.*"}[1m]))/sum(irate(istio_requests_total{reporter="source",destination_service="spring-boot-helloworld.default.svc.cluster.local"}[1m]))
```


#### Rollout

```bash
kubectl argo rollouts set image rollouts-helloworld-with-analysis spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.6 -n demo 
```
