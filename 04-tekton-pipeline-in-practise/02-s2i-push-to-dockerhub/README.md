# Source to Image 

### Kaniko

![kaniko logo](logo/Kaniko-Logo.png)

kaniko is a tool to build container images from a Dockerfile, inside a container or Kubernetes cluster.

kaniko doesn't depend on a Docker daemon and executes each command within a Dockerfile completely in userspace.
This enables building container images in environments that can't easily or securely run a Docker daemon, such as a standard Kubernetes cluster.

kaniko is meant to be run as an image: `gcr.io/kaniko-project/executor`.

#### Pushing to Docker Hub

Get your docker registry user and password encoded in base64

    echo -n USER:PASSWORD | base64

Create a `config.json` file with your Docker registry url and the previous generated base64 string

**Note:** Please use v1 endpoint. See #1209 for more details

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
        kubectl create secret generic docker-config --from-file=<path to .docker/config.json>
        ```

    **Note:** The type of secret must use generic, and must **not** be docker-registry.
