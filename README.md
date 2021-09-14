# Kubernetes Metrics Server

In our example we have installed metric-server by loging into the Master Server and cloning the git to get required deployment files.
root@Master-Node:~# su - kubeadm
$ yum install -y git
$ git clone https://github.com/omeshwarkandari/metric-server.git
$ cd metric-server/deploy/1.8+
$ ls
aggregated-metrics-reader.yaml  auth-reader.yaml         metrics-server-deployment.yaml  resource-reader.yaml
auth-delegator.yaml             metrics-apiservice.yaml  metrics-server-service.yaml
$ kubectl apply -f .
It will run these yaml files and create metrics-server pod in the kube-system namespaces.
$ kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS       AGE
calico-kube-controllers-58497c65d5-mx4lr   1/1     Running   4 (40m ago)    10d
calico-node-2jwqb                          1/1     Running   4 (40m ago)    10d
calico-node-5mt49                          1/1     Running   4 (21h ago)    10d
calico-node-gtbrk                          1/1     Running   4 (40m ago)    10d
coredns-78fcd69978-bkc9z                   1/1     Running   4 (21h ago)    10d
coredns-78fcd69978-bwfjx                   1/1     Running   4 (21h ago)    10d
etcd-master-node                           1/1     Running   10 (21h ago)   10d
kube-apiserver-master-node                 1/1     Running   12 (21h ago)   10d
kube-controller-manager-master-node        1/1     Running   12 (40m ago)   10d
kube-proxy-krcd9                           1/1     Running   4 (21h ago)    10d
kube-proxy-n5fvp                           1/1     Running   4 (40m ago)    10d
kube-proxy-rsltn                           1/1     Running   4 (40m ago)    10d
kube-scheduler-master-node                 1/1     Running   12 (21h ago)   10d
metrics-server-78b554f76c-grpn9            1/1     Running   0              24m

Once metrics-server pod is created we can run the below commands:
Metrics of the nodes:
$ kubectl top nodes
NAME          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
master-node   327m         16%    1250Mi          33%
node-1        95m          4%     927Mi           24%
node-2        89m          4%     615Mi           16%

Metrics of the application pod:
$ kubectl top pods
NAME                                    CPU(cores)   MEMORY(bytes)
petclinic-deployment-7568f578d9-s6jnz   1m           252Mi





## User guide

You can find the user guide in
[the official Kubernetes documentation](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/).

## Design

The detailed design of the project can be found in the following docs:

- [Metrics API](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/resource-metrics-api.md)
- [Metrics Server](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/metrics-server.md)

For the broader view of monitoring in Kubernetes take a look into
[Monitoring architecture](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/monitoring_architecture.md)

## Deployment

Compatibility matrix:

Metrics Server | Metrics API group/version | Supported Kubernetes version
---------------|---------------------------|-----------------------------
0.3.x          | `metrics.k8s.io/v1beta1`  | 1.8+
0.2.x          | `metrics.k8s.io/v1beta1`  | 1.8+
0.1.x          | `metrics/v1alpha1`        | 1.7


In order to deploy metrics-server in your cluster run the following command from
the top-level directory of this repository:

```console
# Kubernetes 1.7
$ kubectl create -f deploy/1.7/

# Kubernetes > 1.8
$ kubectl create -f deploy/1.8+/
```

You can also use this helm chart to deploy the metric-server in your cluster (This isn't supported by the metrics-server maintainers): https://github.com/helm/charts/tree/master/stable/metrics-server




