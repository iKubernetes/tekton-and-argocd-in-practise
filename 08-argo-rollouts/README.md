# Argo Rollouts Examples.

### 01-argo-rollouts-demo.yaml

kubectl apply -f 01-argo-rollouts-demo.yaml -n demo

#### rollout

kubectl argo rollouts set image rollouts-spring-boot-helloworld spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.5

#### watch

```bash
kubectl argo rollouts get rollout rollouts-spring-boot-helloworld --watch
```

#### Promote

```bash
kubectl argo rollouts promote rollouts-spring-boot-helloworld
```

### 02-argo-rollouts-with-istio-traffic-shifting.yaml

**Dependicies**: Istio

```bash
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
kubectl argo rollouts set image rollouts-helloworld-with-traffic-shifting spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.5 -n demo
```

### 03-argo-rollouts-with-analysis.yaml

**Dependicies**: Istio and Prometheus

```bash
kubectl apply -f 02-argo-rollouts-with-istio-traffic-shifting.yaml -n demo
```

#### Rollout

```bash
kubectl argo rollouts set image rollouts-helloworld-with-analysis spring-boot-helloworld=ikubernetes/spring-boot-helloworld:v0.9.5 -n demo 
```