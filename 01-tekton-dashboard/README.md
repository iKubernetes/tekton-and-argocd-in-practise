# Tekton Dashboard



> 前提：
>
> 1. 首先根据Tekton的文档，部署Tekton Pipelines和Tekton Dashboard；
>
>    ```
>    # Tekton Pipelines
>    kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
>    
>    # Tekton Dashboard
>    kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml
>    ```
>
>    
>
> 2. 根据Project Contour的文档，部署Contour，并启用Gateway API，文档地址如下（这里以v1.30版本为例）；
>
>    ```
>    https://projectcontour.io/docs/1.30/guides/gateway-api/
>    ```
>
>    根据文档提示，在Contour Gateway API的两种部署选项中选择其中一种即可。



本目录中的示例提供了三种暴露Tekton Dashboard至集群外部的方式，根据需要选择其中之一即可。

- 基于Ingress：默认使用的是Contour Ingress Class
- 基于Kubernetes Gateway：默认使用的是Contour Gateway
- 基于Istio Gateway



另外，获取Tekton CLI的地址为https://github.com/tektoncd/cli/blob/main/README.md。



