# NFS CSI driver for Kubernetes

### Overview

NFS CSI driver is a project for [NFS](https://en.wikipedia.org/wiki/Network_File_System) [CSI](https://kubernetes-csi.github.io/docs/) driver, csi plugin name: `nfs.csi.k8s.io`. This driver requires existing and already configured NFSv3 or NFSv4 server, it supports dynamic provisioning of Persistent Volumes via Persistent Volume Claims by creating a new sub directory under NFS server.

### Project status: GA

### Container Images & Kubernetes Compatibility:
|driver version  | supported k8s version | status |
|----------------|-----------------------|--------|
|master branch   | 1.20+                 | GA     |
|v4.1.0          | 1.20+                 | GA     |
|v4.0.0          | 1.20+                 | GA     |
|v3.1.0          | 1.19+                 | beta   |

### Install driver on a Kubernetes cluster
 - install by [kubectl](https://github.com/kubernetes-csi/csi-driver-nfs/docs/install-nfs-csi-driver.md)
 - install by [helm charts](https://github.com/kubernetes-csi/csi-driver-nfs/charts)

### Driver parameters
Please refer to [`nfs.csi.k8s.io` driver parameters](https://github.com/kubernetes-csi/csi-driver-nfs/docs/driver-parameters.md)

### Examples
 - [Basic usage](https://github.com/kubernetes-csi/csi-driver-nfs/deploy/example/README.md)
 - [fsGroupPolicy](https://github.com/kubernetes-csi/csi-driver-nfs/deploy/example/fsgroup)

