Tracking my learning of Kubernetes (https://kubernetes.io).

### Setup for my local development environment:

Using *Minikube* to run a k8s environment locally.

# Kubernetes components

Some of the core components of Kubernetes are:

1. `kube-apiserver`: exposes a REST API for management; consumes JSON via manifest files.

2. `Cluster store`: has persistent storage; stores the cluster state and config; uses `etcd` currently as the "source of truth" for the cluster. It aims to be distributed, consistent, and watchable.

3. `kube-controller-manager`: "a controller of controllers" that watches for state changes. Its job is to maintain the desired state. Examples of underlying controllers are a node controller, endpoints controller, namespace controller, etc.

4. `kube-scheduler`: watches *kube-apiserver* for new Pods and assigns work to nodes (handles affinity/anti-affinity, constraints, and resources, amongst much more).

5. `Kubelet`: the main k8s agent. Registers nodes with clusters, watches the `apiserver` and handles the instantiation of Pods. It reports back to 'master' and exposes an endpoint on :10255 by default.

    Some endpoints are:
    * /spec
    * /healthz
    * /pods

6. `Container Engine`: handles container management (i.e. pulling/building images, starting/stopping containers, etc.). Can specify different container runtimes to use here such as *Docker*, *rkt*, etc.

7. `kube-proxy`: handles the k8s networking. Assings Pod IP addresses (all containers in a Pod share the same IP address). The kube-proxy also handles load-balancing across all Pods in a service.

## Pods

    The fundamental unit of scheduling. Similar to running a container in Docker; more specifically, Pods run one or more container(s).

    To instantiate Pods, declare Pods in a manifest .yml file. The apiserver will handle instantiation/deployment based on the manifest file, utilizing the kube-scheduler to execute the deployment.

## Replication Controllers

    Replication Controllers specify Pods and implements their desired state.

## Replica Sets

    Creates and destroys Pods dynamically.

## Services

    An abstraction that defines a logical set of Pods and corresponding policy by which to access them. The set of Pods targeted by a Service is typically determined by a `Label Selector` (see k8s docs).

    The value of using Services is that this abstraction allows for decoupling between different components, i.e. frontend and backend. For example, a frontend service should not care which particular backend instantiation is used, and should not need to maintain awareness or keep a list of available backend instantiations themselves (since backend components may terminate, with replacement backend instantiations automatically being spun up to replace them, etc.). This service-oriented approach will find a backend available/suitable for the frontend to service the request.
