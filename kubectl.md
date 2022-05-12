kubectl is the official command-line tool used ny kubernetes.

This is an http client that is fully optimized to interact with kubernetes and allows you to issue commands to your kubernetes clusters.

### Role of kubectl

client to communicate with kube-apiserver
when you call kubectl, it reads the parameters you pass to it, and based on them, will forge and issue HTTP, requests to kube-apiserver component of your kubernetes cluster.

once the kube-apiserver component recieves a valid http request coming from you, it will read or update the state of the cluster in `Etcd` based on the request you submitted.

### How does kubectl work?

When ypu call kubectl command, it will try to read a configuration file that must be
created in `$HOME/.kube/CONFIG`.
This configuration file is called `kubeconfig`
All of the information, such as the `kube-apiserver` end point, it's port, and the
client certificate used to authenticate against `kube-apiserver`, must be written in this file.
It's path can be overridden on your system by setting an environment variable, called `KUBECONFIG`, or by using the `--kubeconfig` parameter when calling `kubectl`

`$export KUBECONFIG="/custom/path/.kube/config"`
`$kubectl --kubeconfig="/custom/path/.kube/config"`

Each time you run a `kubectl` command, the `kubextl` command-line tool will look for a `kubeconfig` file in which to load its configuration in the following order:

1. Check whether the --kubeconfig parameter has been passed and loads the config file.
2. At that point if no `kubeconfig` file is found, kubectl looks for the KUBECONFIG env variable.
3. Ultimately, it fals back to the default one in `$HOME/.KUBE/CONFIG`

To view the config file currently used by your local kubectl

<br>

| Component Name            | Communicates With                                                                         | Role                                                                                                                                                                                          |
| ------------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kube-apiserver            | kubectl client(s), etcd, kube-scheduler, kube-controller-manager, kubelet, and kube-proxy | The HTTP REST API. It can read and write the state stored in Etcd. The only componet that is able to communicate with Etcd directly                                                           |
| Etcd                      | kube-apiserver                                                                            | This stores the state of the kubernetes cluster                                                                                                                                               |
| kube-scheduler            | kube-apiserver                                                                            | This reads the API every 20 seconds to list unscheduled pods (an empty nodename property), elects a worker node, and updates the nodename property in the pod entry by calling kube-apiserver |
| kube-controller manger    | kube-apiserver                                                                            | This polls the API and runs the reconcliation loops                                                                                                                                           |
| kubelet                   | kube-apiserver and docker (container run time)                                            | This reads the API every 20 seconds to get pods scheduled to the node it's running on, and translates the pod specs into running containers by calling the local deamon.                      |
| kube-proxy                | kube-apiserver                                                                            | This implements the networking layer of kubernetes                                                                                                                                            |
| container engine (docker) | kubelet                                                                                   | This runs the container by recieving instructions from the local kubelet                                                                                                                      |

`minikube start --driver=docker`

`minikube status`

`kubectl config view`

`kubectl config current-context`

`kubectl get nodes`

`kubectl get componentstatuses` same as `kubectl get cs`

`minikube stop`

`minikube delete`

### Launching our first pod.

#### Creating pod with imperative syntax

- Need two parameters to create a pod. <br>
- Pod's name defined by you. <br>
- The Docker image(s) to build it's underlying container(s).
  `kubectl run nginx-Pod --image nginx:latest`
  `kubectl get pods`
  `kubectl describe pods podname`

### Listing the objetcs in JSON or YAML

USE `-O` option
listing ans saving to a file `kubectl get pods/pod-name -o yaml > filename.yaml`

### Getting more information from the list operation

`kubectl get pods -o wide`

### Accessing a pod from the outside world

`kubectl port-forward Pod/nginx-Pods 8080:80`

### Entering a container inside a pod

`$ kubectl exec -ti Pods nginx-Pod bash`

### Delete a pod

`kubectl delete pods pod-name`

### List pods using labels

`kubectl get pods -l environment=production`

### Listing labels attached to a pod

`kubectl get pods --show-labels`

### Adding or updating a label to/of a runnign pod

`kubectl label pods nginx-pod stack=blue`

### Deleting a label attached to a running pod

`kubectl label nginx-pod stack-`

### Adding an annotation
