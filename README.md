# Enable cifs via flexVolume in kubernetes

## Motivation

To use an CIFS (Common Internet File Systen) volume in Kubernetes Pod.

## How-to

### How to mount an NFS share in Pod?

[NFS volume](https://kubernetes.io/docs/concepts/storage/volumes/#nfs) is natively supported in Kubernetes.

### How to mount an CIFS share in Pod?

This [article](https://stackoverflow.com/a/59406874) recommends using [FlexVolume](https://kubernetes.io/docs/concepts/storage/volumes/#flexvolume). There is an existing [driver](https://github.com/fstab/cifs) for CIFS.

### How to install packages to Kubernetes nodes in a Kubernetes way, without engaging your IaaS operator?

Schedule a [priviliged](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#privileged) Pod to each node via [Daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/). Use [nsenter](https://man7.org/linux/man-pages/man1/nsenter.1.html) to run package-intall script in the host namespace.

Some examples:

* [Initialize your AKS nodes with DaemonSets](https://medium.com/@patnaikshekhar/initialize-your-aks-nodes-with-daemonsets-679fa81fd20e)

* [Bad Pods: Kubernetes Pod Privilege Escalation](https://labs.bishopfox.com/tech-blog/bad-pods-kubernetes-pod-privilege-escalation)


## Procedure

```
kubectl apply -f ./manifests-to-install-cifs-driver

# verify if the drive is installed successfully
kubectl logs init-cifs-<pod-name-hash> -c init-cifs

# verify if the cifs share is mounted in the testing pod
kubectl exec ubuntu-cifs-mounted -it -- ls /data/lab-cifs
```
