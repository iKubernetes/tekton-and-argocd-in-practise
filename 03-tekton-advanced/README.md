# Pipeline and Task

为测试环境准备StorageClass，本示例以OpenEBS项目提供的openebs-hostpath为例。

### 部署OpenEBS 3.10.x

请参考[这篇文档](https://github.com/iKubernetes/learning-k8s/tree/master/OpenEBS)中的说明。本示例中的maven-cache需要用到“RWX”的访问模式，建议同时部署“OpenEBS Dynamic NFS Provider”。

### 部署OpenEBS 4.x

```bash
helm install openebs --namespace openebs openebs/openebs --set engines.replicated.mayastor.enabled=false \
            --set engines.local.zfs.enabled=false --create-namespace
```



