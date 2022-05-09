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