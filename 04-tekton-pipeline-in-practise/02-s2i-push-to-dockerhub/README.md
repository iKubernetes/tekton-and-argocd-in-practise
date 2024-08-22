# Source to Image 

### Kaniko

![kaniko logo](https://github.com/GoogleContainerTools/kaniko/blob/main/logo/Kaniko-Logo.png)

kaniko is a tool to build container images from a Dockerfile, inside a container or Kubernetes cluster.

kaniko doesn't depend on a Docker daemon and executes each command within a Dockerfile completely in userspace.
This enables building container images in environments that can't easily or securely run a Docker daemon, such as a standard Kubernetes cluster.

kaniko is meant to be run as an image: `gcr.io/kaniko-project/executor`.

#### Pushing to Docker Hub

Get your docker registry user and password encoded in base64

    echo -n USER:PASSWORD | base64

Create a `config.json` file with your Docker registry url and the previous generated base64 string

> 注意：此处的文件名将作为Secret中的key，其挂载后代表的文件名要求必须为config.json，因此，若使用了其它的文件名，则必须要在创建Secret对象时，将其key明确定义为config.json。

**Note:** 如果是Docker Hub，Please use v1 endpoint，如果是自行部署的私有Registry，例如Harbor，可直接使用服务器地址，形如“https://registry.magedu.com/”即可。

```
{
	"auths": {
		"https://index.docker.io/v1/": {
			"auth": "xxxxxxxxxxxxxxx"
		}
	}
}
```

Configure credentials

    You can create a Kubernetes secret for your `~/.docker/config.json` file so that credentials can be accessed within the cluster.
    To create the secret, run:
        ```shell
        kubectl create secret generic registry-credential --from-file=config.json=<path to .docker/config.json>
        ```
    
    注意： Secret的类型必须为“generic”，一定不能使用“docker-registry”。

