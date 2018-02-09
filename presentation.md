Kubernetes Storage and GlusterFS
===

#  

Docker Storage

Kubernetes Storage

GlusterFS for K8s

# Docker Storage

https://docs.docker.com/storage/

1. within container: inside writable layer of a container
  * deleted with container 
  * couple with host machine
  * require storage driver

  docker ps -s

  docker inspect ubuntu

  dd if=/dev/zero of=1Mfile bs=1k count=1000

2. Docker volume
  * a directory on host
  * prepare: provision on host
  * usage: set volume on docker run

# Kubernetes Storage

https://kubernetes.io/docs/concepts/storage/volumes/

1. On-disk files:
  * Deleted on container restart
  * File sharing in Pod

2. Kubernetes Volume:
  * a directory 
  * Coexist with Pod
  * Data preserved across container restarts
  * Pod can use many volumes of different types

pod.spec.volumes

pod.spec.containers.volumeMounts

(Some of) Types of volumes :
  * emptyDir
    - first created volume
    - prepare: none
    - usage: always

  * gcePersistentDisk
    - independent to pod
    - prepare: gcp
    - usage: claim by name

    ```
    gcloud compute disks create --size=500GB --zone=us-central1-a my-data-disk
    ```

  * PersistentVolumeClaim
    - prepare: provision by admin
    - usage: add PVC request


  [Example](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

### PersistentVolume 
  
  [Doc](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

    - a piece of provisioned storage
    - Independent lifecycle
    - abstract with k8s object API
    - many implementations: ex. GCEPersistentDisk, NFS, GlusterFS...

  * Why PersistentVolume
    - one APIs, many PV implementations
    - Separates providers (admin) and consumers (users)
    - PV subsystem API handles details of implementation
    - Handle different need like size, access mode, performance...
  
  * PersistentVolumeClaim
    - PV: a resource 
    - PVC: a request for storage
    - Pods consume Node resources and PVCs consume PV resources 

  * PVC lifecycle
    - Povisioning
    - Binding
    - Using
    - Reclaiming
    - Deleting

  * PV Access Modes
    - ReadWriteOnce: 1 node R/W
    - ReadOnlyMany: n node R, 1 node W
    - ReadWriteMany: n node R/W

  * StorageClass
    - 
    - usage:PV.storageClassName

  [Doc](https://kubernetes.io/docs/concepts/storage/storage-classes/)

# GlusterFS

  * On pod delete: unmount

    networked filesystem

# GlusterFS for k8s

  * Heketi
    - REST storage management API

# Demo  

  * Env
    - Kubernetes 1.9.2
