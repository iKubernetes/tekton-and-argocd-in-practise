# Pipeline案例环境说明

示例中的用到的[项目](https://gitee.com/mageedu/spring-boot-helloWorld.git)，其Dockerfile用到的Image需要从Dockerhub中拉取，建议将其修改为国内便于访问的地址，例如本地部署的Harbor上的镜像，这样也便于在后续的步骤中进行Image Push的测试。

另外，在运行本示例中的代码之前，依赖于事先存在的名为maven-cache的PVC，若不存在，可基于类似如下的配置创建。

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: maven-cache
spec:
  # 指定的StorageClass，需要事先存在
  storageClassName: openebs-rwx
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
```

