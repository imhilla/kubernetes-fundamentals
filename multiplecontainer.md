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