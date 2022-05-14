### Using multiple container pods and design patterns

Runnig complex applications on kubernetes will require that you run not one but several containers in the same pod.

### Topics

- Understanding what multi-container pods are.
- Sharing volumes between containers in the same pod.
- The ambassador desing pattern.
- The sidecar design pattern.
- The adapter design pattern.

### Understanding what multi-container pods are.

You should group containers into a pod when they are tightly linked.
A pod must correspond to an application or a process running in your kubernetes cluster.
If your application requires multiple containers to function properly then
those containers should be launched and managed through a singlr pod.
When the containers are supposed to work together, you should group them into a single pod. A pod cannot span across multiple worker nodes.
So fi you create a pod containing several containers then all these containers will be created on the same workder node and same docker deamon installation.

### When not to create a multi-container pod.

Pods are especially useful when they are managing several containers, but they should not be seen as the go-to solution every time you need to set up a container.

### creating a pod made up of two containers

`kubectl create -f multi-container-pod`

This will result in pod being created. The kubelet on the elected worker node will have docker deamon to pull both images and instantiate two docker containers.

### Deleting a multi-container

To delete a multi-container pod you have to go through the `kubectl delete` command, just like you would for a single container pod.

Then you have two choices

- You specify the path to the YAML manifest file that's used by using the `-f` option.
  `kubectl delete -f multi-container-pods.yaml`
  You delete the pod without using its YAML PATH if you know it's name.
  `kubectl delete pods/nginx-pod` or `kubectl delete pods nginx-pod`

### Accessing a specific container inside a multi-container pod.

To access a running container, you need to use the `kubectl exec` command, just like you need to use `docker exec` to launch a command in an already created container when using docker with kubernetes.
The command will ask for two important parameters.

- The pod that wraps the container you want to get
- The name of the container itself, as entered in the YAML manifest file.

First, we will have to use `kubectl describe pods/multi-container-pod` command to find out what is contained in this pod.

This command will show you the names of all containers in the targeted pod.
Access a single container.
use `kubectl exec -ti multi-container-pod --container nginx-container -- /bin/bash`
`kubectl exec` does the same as `docker exec`

### Running commands in containers

To run a command in a container, you need to use `kubectl exec`
`kubectl exec pods/multi-container-pod --container nginx-container -- ls`
This command will ask you for two parameters

- The name of the container
- The name of the pod

### Overriding the default commands run by your containers

Docker files make use of two keywords to tell us what commands and arguments
the containers that were built with this image will launch when they are created using the command `docker run` command.
The keywords are
`ENTRYPOINT` and `CMD`
Entry point is the main command the docker container will launch.
CMD is used to replace the parameters that are passed to the ENTRYPOINT command.

Kubernetes allows us to override both ENTRYPOINT and CMD thanks to YAML pod definition file.
To do so, you must append two optional keys to your YAML configuration file: command and args

### introducing the initContainers

initContainers is a feature provided by kubernetes pods to run setup script before the actual container starts.
They will run first when the pod is created, then once they are complete, the pods start creating its main containers.
In general initContainers are used to pull application code from a git repository and expose it to the main containers using volume mounts or to run start-up scripts.
`kubectl create -f nginx-with-init-container.yaml`

### Accessing the logs of a specific container

The proper way to proceed is by using the `kubectl logs` command
`kubectl logs -f pods/multi-container-pods --container nginx-container`

### Sharing volumes between containers in the same pod

### What are kubernetes volume?

kubernetes has two types of volumes

- Volumes
- persistent volume

### Creating and mounting an emptyDir volume

### Creating and mounting a hostPath volume
