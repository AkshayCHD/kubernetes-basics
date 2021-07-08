# Kubernetes configuration files

The first 2 lines of every configuration file tell us what we want to create, like for deployments it will be like
```
apiVersion: apps/v1
kind: Deployment
```
For service it will be 
```
apiVersion: v1
kind: Service
```

##  3 parts of a Kubernetes config file (metadata, specification, status)
Every configuration file in kubernetes ahs 3 parts.
* metadata: Contains metadata for the configuration like, name and labels
* spec: contains various attributes depending on the ind of component you are creating, like for deployment it will contain `replicas`, `template` etc
* status: this part is maintained by kubernetes. Whenever we make a change to the configuration file kubernetes checks if desirec state != actual state, and then make the changes to actual state. The data about actual state is given by etcd (brain of master node). 

example of a conf file
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


##  blueprint for pods (template)
Now the question is where inside the configuration file do we specify the configuration/blueprint of a pod inside a deployment configuration, so we do that inside spec->template, and as you can see above it has a metadata property and spec property in itseldf, so that is like having a conf file inside a conf file. So a pod should have its own configuration inside a deployment configuration file, which is the blueprint of the pod containing information like which port should it open, which image should it use etc.

##  connecting services to deployments and pods (label & selector & port)

##  demo