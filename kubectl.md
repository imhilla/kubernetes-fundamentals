kubectl is the official command-line tool used ny kubernetes.

This is an http client that is fully optimized to interact with kubernetes and allows you to issue commands to your kubernetes clusters.

### Role of kubectl
client to communicate with kube-apiserver
when you call kubectl, it reads the parameters you pass to it, and based on them, will forge and issue HTTP, requests to kube-apiserver component of your kubernetes cluster.

once the kube-apiserver component recieves a valid http request coming from you, it will read or update the state of the cluster in `Etcd` based on the request you submitted.