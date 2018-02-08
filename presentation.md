Kubernetes Storage and GlusterFS
===

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

bind mounts vs volume


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

(Some of) Types of volumes:
  * emptyDir
    - first created volume

  * configMap
    - storage config data

  * gcePersistentDisk
    - independent to pod

    gcloud compute disks create --size=500GB --zone=us-central1-a my-data-disk

  * hostPath

  * PersistentVolumeClaim

  https://kubernetes.io/docs/concepts/storage/persistent-volumes/

  https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

    - PV: a piece of provisioned storage (abstract)
    - Independent lifecycle
    - PV subsystem API handles details of implementation: like NFS
    - Separates providers (admin) and consumers (users)

    - PVC: a request for storage
    - PVCs consume PV resources and Pods consume Node resources

    - Request size, access mode, performance...
    - StorageClass resource

  * Container storage interface


Persistent volume


# Glusterfs

  * On pod delete: unmount

    networked filesystem


