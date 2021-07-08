## Kubectl commands

## Get status of different components
To get status of a Node
```
kubectl get nodes
```

To get status of a pod
```
kubectl get pod
```

To get status of a services
```
kubectl get services
```

## create a pod/deployment
Usually pod is the smallest unit in a kubernetes cluster but in practice you do not create a pod, you create a deployment to manage the pod configuration. You can say that a deployment is basically an abstraction over a pod.
To create a deployment from a docker image we can do

```
kubectl create deployment nginx-depl-kubectl --image=nginx
```

This will give the output
```
NAME                                  READY   STATUS              RESTARTS   AGE
nginx-depl-kubectl-5bbcdc84cc-g2lp7   0/1     ContainerCreating   0          5s
```

Now there is another entity in kubernetes, which are replicasets. So the pod that we created above is part of a replca set whose Id is `5bbcdc84cc` and the id of the pod is `g2lp7`

and when we type 

```
kubectl get replicasets
```
we get 
```
NAME                            DESIRED   CURRENT   READY   AGE
nginx-depl-kubectl-5bbcdc84cc   1         1         1       3m59s
```
In this deployment we specified only 1 replica of the pod.

## layers of abstraction
So this is the layer of abstraction   
Deployment -> ReplicaSets -> Pods -> Containers   
And here everything below the deployments is usually managed by kubernetes.

## change the pod/deployment
To edit a Pod or replicaset we will have to edit the deployment
```
kubectl edit deployment nginx-depl-kubectl
```
After running the above command we will be shown a yaml confiuration file, and if we edit and save it, a new replicaset will be created and the old pod in the old replicaset will be deleted. 

```
kubectl get pod
```

will give 
```
NAME                                  READY   STATUS    RESTARTS   AGE
nginx-depl-kubectl-5c9878c688-85tdr   1/1     Running   0          3m19s
```

and
```
kubectl get replicaset
```
```
NAME                            DESIRED   CURRENT   READY   AGE
nginx-depl-kubectl-5bbcdc84cc   0         0         0       24m
nginx-depl-kubectl-5c9878c688   1         1         1       3m52s
```

## debugging pods
To know the changes that are happening inside a pod you can do 
```
kubectl describe pod mongo-depl-kubectl
```
And events will be like 
```
Events:
Type    Reason     Age   From               Message
----    ------     ----  ----               -------
Normal  Scheduled  100s  default-scheduler  Successfully assigned default/mongo-depl-kubectl-54678f769c-fx59n to minikube
Normal  Pulling    91s   kubelet            Pulling image "mongo"
```
To see the logs of a container we can use 
```
kubectl logs mongo-depl-kubectl-54678f769c-fx59n
```

Now to enter a container, via a terminal shell, we can use the following command

```
kubectl exec -it mongo-depl-kubectl-54678f769c-fx59n -- bin/bash
```
This can be very usefull while project setup and debugging 

## delete pod/deployment
To delete a deployment you can use, deleting the deployment means deleting the entire replicset and thus all the pods in it.
```
kubectl delete deployment mongo-depl-kubectl
```

## CRUD by applying configuration file
It is a tideous process to remember commands for deploying same configurations, so we can define configurations in a configuration file and use that using the following command

```
kubectl apply -f ./configs/nginx-config.yaml
```
nginx-config.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-depl
spec:
  selector: 
    matchLabels:
      app: nginx-depl
  template:
    metadata:
      labels:
        app: nginx-depl
    spec:
      containers:
      - name: nginx-depl
        image: nginx
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 80
```

Now after running
```
kubectl get pods
```

```
NAME                          READY   STATUS              RESTARTS   AGE
nginx-depl-86b8689f6f-j4jgv   0/1     ContainerCreating   0          8s
```

Now if we want to make any changes to the deployment we can simply change the configuration file and run the apply command again, for example if we want to create 2 replicas we can simply add a replica fiends and run apply command and we'll see 
```
NAME                          READY   STATUS              RESTARTS   AGE
nginx-depl-86b8689f6f-6n9p9   0/1     ContainerCreating   0          6s
nginx-depl-86b8689f6f-j4jgv   1/1     Running             0          3m45s
```

Conf
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-depl
spec:
  replicas: 2
  selector: 
    matchLabels:
      app: nginx-depl
  template:
    metadata:
      labels:
        app: nginx-depl
    spec:
      containers:
      - name: nginx-depl
        image: nginx
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 80

```