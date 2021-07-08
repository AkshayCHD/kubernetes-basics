# Minicube and cubectl

## Minicube
Setting up and testing a kubernetes cluster is a resource intensive process, which involves alot of resources. Which gave rise to minicube which is an open source process that, installs a VM on your system in which you can run a single node in which all master and worker process can run together. It is for testing purposes.

## Kubectl
Is a command line tool that helps you interact with kubernetes cluster. Basically to perform any action in a kubernetes cluster you need to interact with the Api server(which is a master process), and there are 3 ways to do that
* UI
* API
* CLI(Kubectl and the most powerful one, and is not specefic to minicube but can be used to actual cloud based kubernetes cluster)

Minicube is used to startup nodes and kubectl is used to interact with it and perform various operations on it.

 ## Installation

[minikube](https://minikube.sigs.k8s.io/docs/start/)   
[Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) 