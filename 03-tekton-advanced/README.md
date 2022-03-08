# Pipeline and Task

## NFS-CSI

### Install NFS Server
```
kubectl create -f https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/deploy/example/nfs-provisioner/nfs-server.yaml
```

### Install NFS-CSI Driver
```
curl -skSL https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/deploy/install-driver.sh | bash -s master --
```

check pods status
```
kubectl -n kube-system get pod -o wide -l app=csi-nfs-controller
kubectl -n kube-system get pod -o wide -l app=csi-nfs-node
```

### Storage Class Usage (Dynamic Provisioning)

Follow the following command to create a StorageClass, and then PersistentVolume and PersistentVolumeClaim dynamically.

```
# create StorageClass
kubectl create -f https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/deploy/example/storageclass-nfs.yaml

# create PVC
kubectl create -f https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/deploy/example/pvc-nfs-csi-dynamic.yaml
```

### PV/PVC Usage (Static Provisioning)

Follow the following command to create PersistentVolume and PersistentVolumeClaim statically.

```
# create PV
kubectl create -f https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/deploy/example/pv-nfs-csi.yaml

# create PVC
kubectl create -f https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/deploy/example/pvc-nfs-csi-static.yaml
```
