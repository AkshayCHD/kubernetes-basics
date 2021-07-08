# Main K8 Components

## Node 
* A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster
* Each Node is managed by the Master. A Node can have multiple pods, and the Kubernetes master automatically handles scheduling the pods across the Nodes in the cluster
* Basically you can thing of a node as a complete virtual machine.

## Pod
* Smallest unit in kubernetes, basically is an abstraction over a container, because kubernetes does not want us to be dependent on the container technology that we use, we can easily replace that in case we want.
* usually we have One container per Pod
* each pod has its own Ip address
* pods can easily die and recreated, but each time this happens a new Ip(private) is assigned to it, which is not right as now the other pods have to be readjusted to communicate with this pod. This is where services come into play.

## Services
* Service is basically a static Ip address that can be attached to each pod.
* lifecycle of service and pod are not related so, even if a pod die we have to make no change to service 
* Services also act as a load balancer, when the cluster scales up the nodes, services balances the load between the same type of pods existing between different nodes.
* Suppose a pod in a node is down due to some reason then the service will direct all requests to that pod to a same kind of pod in different node, which results in high availability.
* Now there are 2 types of services internal and external(For the application to communicate with external world), but the format of service is like `http://ip-address:port` so it is not a good practice to expose such a url outside. This is where Ingress comes into picture.

## Ingress
* Ingress is basically what communicates with the external world and forwards the request to the services.
* Ingress routes traffic into the cluster

## Config Map
* It is used to store the external configuration for a pod, suppose we want to change the db url string, in it is hardcoded in the code, we will have to push updated code to github then pull it to k8 then restart the pod completely.
* Now if we have a db_url variable inside the pod, it can be changed easily.

## Secrets
* Config map stores variables in plain text format so cannot be used to store credentials like passwords, certificate files etc, so for that we use another k8 component, secrets store data in `base-64` incoded format

## Volumes
* As mentioned previously, pods and get restarted due to several reasons ni k8, in that case any data stored inside the pod, will be lost, as k8 cluster does not handle data persistence in any way itself, here comes the use of volumes.
* Volumes basically attaches a hardware storage to your pod, which can be on the same machine or on a remote location.
* So volumes act as a connecter between k8 pod and external persistent storage

## Deployments
* Deployments are basically blueprints for a k8 pod
* In k8, we dont directly work with pods but with deployments, we specify the congifuration for a pod inside the deployment, and the deployment is then responsible to scale up and down the pod, and other sucb configurations
* Like pods are an abstraction to containers, deployments are an abstraction to pods.

## Stateful set
* They are like deployments only but have the additional functionality of keeping the requests synchronized.
* Like in database applications we need to keep the requests synchronised in order to prevent data inconsistencies.
* But it is hard to implement, thus kubernetes are mostly used for deploying stateless applications.