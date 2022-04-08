# Argo Rollouts Examples.

### 01-argo-rollouts-demo.yaml

```bash
kubectl create namespace demo
kubectl apply -f 01-argo-rollouts-demo.yaml -n demo
```

#### client

```bash
kubectl run client-$RANDOM --image ikubernetes/admin-box:v1.2 --rm -it --restart=Never --command -- /bin/bash
```

send requests...
```bash
while true; do curl http://spring-boot-helloworld.demo.svc.cluster.local; echo; sleep 1; done
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
export ISTIO_DIR=/usr/local/istio/
kubectl apply -f ${ISTIO_DIR}/samples/sleep/sleep.yaml
export SLEEP=$(kubectl get pods -l app=sleep -o jsonpath='{.items[0].metadata.name}')
kubectl exec -it $SLEEP -- /bin/sh
```

send requests...
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
#### client

```bash
export ISTIO_DIR=/usr/local/istio/
kubectl apply -f ${ISTIO_DIR}/samples/sleep/sleep.yaml
export SLEEP=$(kubectl get pods -l app=sleep -o jsonpath='{.items[0].metadata.name}')
kubectl exec -it $SLEEP -- /bin/sh
```

```bash
while true; do curl http://spring-boot-helloworld.demo.svc.cluster.local; echo; sleep 1; done
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

### Blue Green Rollout

#### Deploy

```bash
kubectl apply -f 05-argo-rollouts-bluegreen-demo.yaml -n demo
```

#### client

```bash
kubectl run client-$RANDOM --image ikubernetes/admin-box:v1.2 --rm -it --restart=Never --command -- /bin/bash
```

send requests...
```bash
while true; do curl http://spring-boot-helloworld.demo.svc.cluster.local; echo; sleep 1; done
```

#### Rollout

```bash
kubectl argo rollouts set image rollout-helloworld-bluegreen spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.6 -n demo
```

#### Promote

```bash
kubectl argo rollouts promote rollout-helloworld-bluegreen -n demo
```

### Blue Green Rollout with Analysis

#### Deploy

```bash
kubectl apply -f 06-argo-rollouts-bluegreen-with-analysis.yaml -n demo
```

#### client

```bash
kubectl run client-$RANDOM --image ikubernetes/admin-box:v1.2 --rm -it --restart=Never --command -- /bin/bash
```

send requests...
```bash
while true; do curl http://spring-boot-helloworld.demo.svc.cluster.local; echo; sleep 1; done
```

#### Rollout

```bash
kubectl argo rollouts set image rollout-helloworld-bluegreen-with-analysis spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.6 -n demo
```
