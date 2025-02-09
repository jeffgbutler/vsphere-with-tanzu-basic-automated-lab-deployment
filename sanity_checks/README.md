## Great series of YouTube Videos:

- https://www.youtube.com/watch?v=uSGujnlYpVc - Deep Dive 1: Network Prerequisites and Routing
- https://www.youtube.com/watch?v=Ubsm04VWXKk - Deep Dive 2: Creating a Nested Cluster
- https://www.youtube.com/watch?v=wfYDDbBJHfM - Deep Dive 3: Configuring a vSphere Cluster for Tanzu
- https://www.youtube.com/watch?v=biqSFIu_hmQ - Deep Dive 4: Creating a Namespace and Deploying a K8S Cluster
- https://www.youtube.com/watch?v=rWNYPRxihno - Deep Dive 5: Exploring a Tanzu Kubernetes Cluster

Corresponding GitHub Repo: https://github.com/corrieb/vspherewithtanzudemo/tree/main/sanity

## Setup:

1. Create a namespace "primp-industries"
1. Login with `kubectl vsphere login --server 192.168.137.64 -u administrator@vsphere.local --insecure-skip-tls-verify`

## Interesting Commands:

1. `kubectl config get-contexts` will show two contexts - the IP, and primp-industries
1. `kubectl config use-context primp-industries`
1. `kubectl describe virtualmachineclasses` - shows virtual machins classes available
1. `kubectl describe ns primp-industries` - shows storage classes available
1. `kubectl get storageclasses` - also shows storage classes
1. `kubectl get TanzuKubernetesReleases` - shows what Kubernetes versions are available
1. `kubectl get VirtualMachineImages` - shows virtual machine images which is similar to Kubernetes versions, but doesn't show upgrade paths
1. `kubectl vsphere logout`

## Create a Cluster:

1. `kubectl apply -f 00-createcluster.yaml` (took about 25 minutes)
1. `kubectl get TanzuKubernetesClusters` watch progress of cluster creation

## Security

You have very little authority in the supervisor cluster - need to get into your own cluster before you can really do
anything. You can see this with `kubectl get clusterroles` - not authorized

Login to the cluster you created:

```
kubectl vsphere login --server 192.168.137.64 --tanzu-kubernetes-cluster-namespace primp-industries \
  --tanzu-kubernetes-cluster-name tkgs-cluster-1 -u administrator@vsphere.local \
  --insecure-skip-tls-verify
```

Switch into your cluster `kubectl config use-context tkgs-cluster1`

Show roles and role bindings:
- `kubectl get clusterroles`
- `kubectl get clusterrolebindings`

## Deploy a Pod

`kubectl run kuard --restart=Never --image=gcr.io/kuar-demo/kuard-amd64:blue`

`kubectl port-forward kuard 8080:8080`

## Deployments

On vSphere with Tanzu we need to give permission to the default service account for deployments to work.

Run `kubectl apply -f 03a-deployment.yaml`

Run `kubectl describe rs` to show the security error

Run `kubectl apply -f 03b-securitypolicy.yaml` to fix the error

Run `kubectl describe rs` to show things working

## Load Balancer Service

Run `kubectl apply -f 05-loadbalancerservice.yaml` to create the service

Run `kubectl get svc` to see the IP address created from HA Proxy

Navigate to the IP Address
