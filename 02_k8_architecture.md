# Kubernetes Architecture

## Worker Nodes
The main component in kubernetes that does all the work are worder nodes, thus also called worker nodes. Each nodes have multiple pods running on it. There are 3 processes that must be installed on a kubernetes node.
* A container Runtime (like docker), to run containers inside application pods.
* Kublet, it is the process that schedules the running of the pods and thus the containers inside them. Kublet interacts with both the node and pods, and is reponsible for starting the pods, assigning resources to it like cpu, ram and storage resources.
* Kube proxy, which is responsible for communicating requests from pods to services and vice versa, in an efficient manner so that the overhead is the least. For eg: If a app pod whats to communicate with a db pod then the service does not redirect request to any db pod in any Node, but to the pod within the same machine if there is.

## Master Nodes
All the managing work in a kubernetes cluster is code by master nodes, like monitoring health of worker nodes, rescheduling restart of a Pod, start a new Node etc. Master Nodes have completly different processes working from worker nodes, which control the cluster state and worker nodes.
* Api server: Act as an entry point to the cluster, acts as a gatekeeper for the cluster performing authentication for all requests such as a request to start a new pod, or to query the number of pods running.
* Scheduler: Gets the requests from the api server, and schedules updation/initiation requests on the worker node intellegently, i.e keeping in mind the recource availablity on the worker nodes, and making the most optimal choice. (Note tha the pods are started by kublet only inside the node, scheduler is reponsible to determine which Node's kublet has to be informed)
* Contol Manager: Detects cluster state changes, such as a crash and if such cases informs the scheduler to restart or add a new pod through a node kublet.
* Etcd: It can be called a cluster brain. It has a key value store, and all the features involving scheduler or control manager work because of its data. It contains information like which resources are available, did the cluster state change, is the cluster healthy etc.

There can be multiple worker and master nodes in a kubernetes cluster.
In terms of resources master node require  less resources and compared to worker node as worker nodes are responsible for actuall application logic execution.