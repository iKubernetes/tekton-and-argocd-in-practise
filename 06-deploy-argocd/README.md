# ArgoCD and ArgoCD Rollouts

Argo CD是面向Kubernetes的声明式GitOps持续交付（CD）工具。

## Argo CD

### 部署Argo CD

部署前，先要获取最新的稳定版本号，其结果是为形如“2.12.2”一类的版本标识。

```
VERSION=$(curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION)
```

而后，部署Argo CD有HA和非HA两种模式，选其一即可。

#### 非HA模式部署:

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v${VERSION}/manifests/install.yaml
```

#### HA模式部署:

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v${VERSION}/manifests/ha/install.yaml
```

### 开放Dashboard

以下两种方式选其一即可。

#### LoadBalancer Service

```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

#### Ingress （Ingress Nginx）

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-server-ingress
  namespace: argocd
  annotations:
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/ssl-passthrough: "true"
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
    #ingress.cilium.io/loadbalancer-mode: 'shared'
    #ingress.cilium.io/service-type: 'LoadBalancer'
    #ingress.cilium.io/tls-passthrough: 'enabled'
    #ingress.cilium.io/force-https: 'enabled'
spec:
  ingressClassName: nginx
  rules:
  - host: argocd.magedu.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: argocd-server
            port:
              name: https
```

### 部署Argo CD CLI

运行如下命令，即可获取、安装最新的稳定版本的argocd cli，注意其中的变量“VERSION”来自于前面获取最新稳定版本标识的命令。

```
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/download/v$VERSION/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```



### 管理员密码

获取部署过程初始化生成的密码。

```bash
argocd admin initial-password -n argocd
```

命令行登录到Argo CD。

```bash
argocd login <ARGOCD_SERVER>
```

如有需要，可修改密码。

```bash
argocd account update-password
```



## Argo Rollouts

### 部署Rollouts

运行如下命令，即可部署Argo Rollouts。

```
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```

运行如下命令，可部署Argo Rollouts Dashboard。

```bash
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/dashboard-install.yaml
```

#### 开放Rollouts Dashboard

下面命令，会基于Ingress Nginx创建一个ingress资源对象来开放rollout，其使用的主机名为“argo-rollouts.magedu.com”。

```bash
kubectl apply -f https://raw.githubusercontent.com/iKubernetes/tekton-and-argocd-in-practise/main/06-deploy-argocd/argo-rollouts/argo-rollouts-ingress.yaml
```

> 提示：访问Rollouts Dashboard时，若要管理指定名称空间下的Rollout，可以在访问的路径上使用“/rollouts/<NAMESPACE>”。



### 安装kubectl argo rollout插件

运行如下命令，即可安装最新稳定版本的kubectl argo rollout插件。

```bash
curl -LO https://github.com/argoproj/argo-rollouts/releases/download/v1.7.2/kubectl-argo-rollouts-linux-amd64
sudo install -m 555 kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts
rm kubectl-argo-rollouts-linux-amd64
```

运行命令进行测试。

```bash
kubectl argo rollouts version
```

