# Cheat Sheet for Kubernetes Commands

[Cheat Sheet for Kubernetes Commands](https://www.howtoforge.com/kubernetes_commands/)

### Cluster Information

Print the client and server version information

kubectl version

Print the supported API resources on the server

kubectl api-resources

Print the supported API versions on the server, in the form of "group/version"

kubectl api-versions

Print the cluster information

kubectl cluster-info

Get the list of the nodes in the cluster

kubectl  get nodes

Get information of the master node

kubectl  get nodes master -o wide

Get detailed information on the master nodes

kubectl  describe  nodes  master

### Configuration Information

Display merged kubeconfig settings

kubectl  config view

View the current context

kubectl  config  current-context

Set the context, here kubernetes-admin@kubernetes is the context name

kubectl config  use-context kubernetes-admin@kubernetes

Display clusters defined in the kubeconfig

kubectl  config get-clusters

Describe one or many contexts

kubectl  config get-contexts

### Namespaces

Get all namespaces

kubectl  get namespaces

Get namespace information in yaml format

kubectl  get namespaces -o yaml

Describe the default namespace

kubectl  describe  namespace default

Create a new namespace

kubectl  create namespace my-namespace

Delete the namespace

kubectl  delete namespace my-namespace

### Pods

Get pods from the current namespace

kubectl get pods

Get pods from all the namespaces

kubectl get pods --all-namespaces

Get pods from the specified namespace

kubectl get pods -namespace=my-namespace

Create a pod 

kubectl  run my-pod-1 --image=nginx:latest --dry-run

see how the pod would be processedAdvertisement

kubectl  run my-pod-1 --image=nginx:latest --dry-run=client

Create a pod in the specified namespace

kubectl  run my-pod-2 --image=nginx:latest --namespace=my-namespace

Create a pod with a label to it

kubectl  run nginx --image=nginx -l --labels=app=test

Get all pods with label output

kubectl get pods --show-labels

Get pods with exapanded/wide output

kubectl  get pods -o wide

List pods in a sorted order

kubectl  get pods --sort-by=.metadata.name

Get logs of the pod

kubectl  logs  my-pod-1

Get pods within the specified namespace with exapanded/wide output

kubectl get pods my-pod-2 --namespace=my-namespace -o wide

Get logs of the pod within the specified namespace

kubectl  logs  my-pod-2 --namespace=my-namespace

Describe the pod

kubectl  describe  pod my-pod-1

Describe the pod within the specified namespace

kubectl describe  pods my-pod-1 --namespace=my-namespace

Delete the pod from the current namespace

kubectl  delete pod my-pod-1

Delete the pod from the specified namespace

kubectl delete  pods my-pod-1 --namespace=my-namespace

### Deployments

Get a list of deployments from the current namespace

kubectl  get deployments

Get a list of deployments from the specified namespace

kubectl  get deployments --namespace=my-namespace

Create a deployment

kubectl  create deployment my-deployment-1 --image=nginx

Get the specified deployment

kubectl  get deployment my-deployment-1

Get the specified deployment with its labels

kubectl  get deployment my-deployment-1 --show-labels

Describe the specified deployment 

kubectl describe  deployments my-deployment-1

Get details of the deployment in yaml format

kubectl  get deployment my-deployment-1 -o yaml

Change image in the existing deployment

kubectl  set image deployment my-deployment-1 nginx=nginx:1.16.1

View rollout history

kubectl rollout history deployment my-deployment-1

Undo a previous rollout

kubectl rollout undo deployment my-deployment-1

Go back the specific version of the rollout history

kubectl rollout undo deployment my-deployment-1 --to-revision=2

Show the status of the rollout

kubectl rollout status deployment my-deployment-1

Restart a resource

kubectl rollout restart deployment my-deployment-1

Scale deployment to 3

kubectl scale --replicas=3 deployment my-deployment-1

Scale from current count to the desired

kubectl scale --current-replicas=3 --replicas=5 deployment my-deployment-1

This will create an HPA (Horizontal Pod Aotuscaler)

kubectl autoscale deployment my-deployment-1 --min=2 --max=10

### Services

First, create a pod with the label app=myapp.

Then:

Create a pod with a label

kubectl run my-pod --image=nginx --labels=app=myapp

Create a service of type NodePort which will use pod's labels for selector but we have to specify the type, so create a definition file first and then create a service

kubectl expose pod my-pod --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml

Create a Service which will have type type NodePort but this will not have selector as my-app

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml

Get services from the current context

kubectl  get service

Get details of the services

kubectl  get service -o wide

Get services with labels on them

kubectl  get service --show-labels

Get services from all the namespaces

kubectl  get services --all-namespaces

Describe the service to know more about it

kubectl  describe  service nginx-service

Get a particular service

kubectl  get  service nginx-service

Delete the service 

kubectl  delete service nginx-service

### Manage Objects from .yaml/.yml files

First, create a definition file for a pod

Create a definition file for pod 

kubectl  run mypod --image=nginx --dry-run=client -o yaml > my-pod.yml

Create an object 

kubectl  create -f my-pod.yml

Delete the object 

kubectl  delete -f my-pod.yml

# Docker and Kubernetes for Java Developers

## Installing Minikube

The Minikube tool source code with all the documentation is available at GitHub at [GitHub - kubernetes/minikube: Run Kubernetes locally](https://github.com/kubernetes/minikube).

## Installing on Mac

```bash
$ brew install minikube
```

## Starting up the local Kubernetes cluster

We're using the local Kubernetes cluster provided by minikube. Start your cluster with:

```markup
$ minikube start 
```

Minikube works on its own virtual machine. Depending on your host OS, you can choose between several virtualization drivers. Currently supported are virtualbox, vmwarefusion, xhyve, hyperv, and kvm (**Kernel-based virtual machine**). The default VM driver is virtual box. You can override this option. This is the example macOS startup command line which uses xhyve:

```markup
$ minikube start --vm-driver=xhyve

$ minikube start --vm-driver=virtualbox
```

When starting Minikube for the first time, you will see it downloading the Minikube ISO, so the process will take a little longer. This is, however, a one-time action. The Minikube configuration will be saved in the .minikube folder in your home directory, for example ~/.minikube on Linux or macOS. On the first run, Minikube will also configure the kubectl command line tool (we will get back to it in a short while) to use the local minikube cluster. This setting is called a kubectl context. It determines which cluster kubectl is interacting with. All available contexts are present in the ~/.kube/config file.

As the cluster is running now and we have the dashboard addon enabled by default, you can take a look at the (still empty) Kubernetes dashboard with the following command:

```markup
$ minikube dashboard
```

It will open your default browser with the URL of the cluster's dashboard:

**![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/0200456b-d035-4b1a-8b70-d0023a17bfdf.png)**

As we have seen on the kubectl describe service rest-example command output, our rest-example service can be accessed within the cluster via port 8080 and the domain name rest-example. In our case, the complete URL of the endpoint would be http://rest-example:8080. However, to be able to execute the service from the outside world, we have used the NodePort mapping, and we know that it was given the port 31141. All we need to call the service is the IP of the cluster. We can get it using the following command:

```markup
$ minikube ip  
```

There's a shortcut for getting to know the externally accessible service URL and a port number. We can use a minikube service command to tell us the exact service address:

```markup
$ minikube service rest-example --url  
```

The output of the previous command will be the service URL with a mapped port number. If you skip the --url switch, minikube will just open the service's URL using your default web browser. This is sometimes handy.

Having the complete URL of the endpoint, we can access the service, using any of the HTTP clients, such as curl, for example:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/b73014b5-0a23-48ba-8bb2-17344896ae8f.png)

## Installing kubectl

[Install Tools | Kubernetes](https://kubernetes.io/docs/tasks/tools/)

kubectl is available for all major platforms. Let's start with macOS installation.

## Installing on Mac

```bash
$ brew install kubectl
```

## Installing on Windows

You can find the list of Windows kubectl releases on GitHub at [Releases · eirslett/kubectl-windows · GitHub](https://github.com/eirslett/kubectl-windows/releases). Similar to Minikube, kubectl is just a single .exe file. At the time of writing this book it's https://github.com/eirslett/kubectl-windows/releases/download/v1.6.3/kubectl.exe. You will need to download the exe file and place in on your system path, to have it available in the command line.

To verify if your local cluster is up and running and kubectl is properly configured, execute the following command:

```markup
$ kubectl cluster-info     
```

To list the nodes we have running in our cluster, execute the get nodes command:

```markup
$ kubectl get nodes     
```

## Creating a service

The service type can have the following values:

- **NodePort**: By specifying a service type of NodePort, we declare to expose the service outside the cluster. The Kubernetes master will allocate a port from a flag-configured range (default: 30000-32767), and each node of the cluster will proxy that port (the same port number on every node) into your service
- **Load balancer**: This would create a load balancer on cloud providers which support external load balancers (for example, on Amazon AWS cloud). This feature is not available when using Minikube
- **Cluster IP**: This would expose the service only within the cluster. This is the default value which will be used if you don't provide another

Having our service.yml file ready, we can create our first Kubernetes service, by executing the following kubectl command:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: rest-example
  labels:
    app: rest-example
    tier: backend
spec:
  type: NodePort
  ports:
  - port: 8080
  selector:
    app: rest-example
    tier: backend
```

```markup
$ kubectl create -f service.yml
```

## Creating a deployment

Before creating a deployment, we need to have our Docker image ready and published to a registry, the same as the Docker Hub for example. Of course, it can also be a private repository hosted in your organization. As you remember from the [Chapter 7](https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/7), *Introduction to Kubernetes*, each Docker container in a Pod has its own image. By default, the kubectl process in a Pod will try to pull each image from the specified registry. You can change this behavior by specifying a value for the imagePullPolicy property in a deployment descriptor. It can have the following values:

- IfNotPresent: With this setting, the image will be pulled from the registry only if not present on the local host
- Never: With this one, kubelet will use only local images

Using locally built images gets a little bit tricky when working with a local Kubernetes cluster. Minikube runs in a separate VM, hence it will not see the images you've built locally using Docker on your machine. There's a workaround for that. You can execute the following command:

```markup
$ eval $(minikube docker-env)  
```

The previous command will actually utilize the Docker daemon running on minikube, and build your image on the Minikube's Docker. This way, the locally built image will be available to the Minikube without pulling from the external registry. This is not very convenient, it is certainly easier to push the Docker image to a remote registry. Let's push our rest-example image into the DockerHub registry.

1. First, we need to log in:

```markup
$ docker login  
```

2. Then, we are going to tag our image using the docker tag command (not that you will need to provide your own DockerHub username instead of $DOCKER_HUB_USER):

```markup
$ docker tag 54529c0ebed7 $DOCKER_HUB_USER/rest-example  
```

3. The final step will be to push our image to Docker Hub using the docker push command:

```markup
$ docker push $DOCKER_HUB_USER/rest-example
```

4. Now that we have an image available in the registry, we need a deployment manifest. It's again a .yaml file, which can look the same as this:

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: rest-example
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: rest-example
        tier: backend
    spec:
      containers:
      - name: rest-example
        image: jotka/rest-example
        imagePullPolicy: IfNotPresent
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
        ports:
        - containerPort: 8080To create this deployment on the cluster using kubectl, you will need to execute the following command, which is exactly the same as when creating a service, with a difference in the filename:
```

```markup
$ kubectl create -f deployment.yml
```

Let's summarize the kubectl commands related to creating resources and getting information about them, with some examples, in a table:

| **Example command**                                   | **Meaning**                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------------- |
| kubectl create -f ./service.yaml                      | Create resource(s)                                                  |
| kubectl create -f ./service.yaml -f ./deployment.yaml | Create from multiple files                                          |
| kubectl create -f ./dir                               | Create resource(s) in all manifest files in the specified directory |
| kubectl create -f https://sampleUrl                   | Create resource(s) from URL                                         |
| kubectl run nginx --image=nginx                       | Start a single instance of nginx                                    |
| Kubectl get pods                                      | Get the documentation for pod                                       |
| kubectl get pods --selector=app=rest-example          | List all the Pods that match the specified label selector           |
| kubectl explain pods                                  | Show details of all Pods                                            |
| kubectl get services                                  | List all created services                                           |
| kubectl explain service                               | Show details of specified service                                   |
| kubectl explain services                              | Show details of all created services                                |
| kubectl get deployments                               | List all created deployments                                        |
| kubectl get deployment                                | Show details of specified service                                   |
| kubectl explain deployment                            | Show details of specified deployment                                |
| kubectl explain deployments                           | Show details of all created deployments                             |
| kubectl get nodes                                     | List all cluster nodes                                              |
| kubectl explain node                                  | Show details of specified node                                      |

Template

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: second-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: second-app
      tier: backend
  template:
    metadata: 
      labels:
        app: second-app
        tier: backend
    spec: 
      containers:
        - name: second-node
          image: academind/kub-first-app:2
        # - name: ...
        #   image: ...
```

Service

```yml
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector: 
    app: second-app
  ports:
    - protocol: 'TCP'
      port: 80
      targetPort: 8080
    # - protocol: 'TCP'
    #   port: 443
    #   targetPort: 443
  type: LoadBalancer
```

```bash
kubectl create deployment first-app --image=kub-first-app
kubectl expose deployment first-app --type=LoadBalancer --port=8080
minikube service first-app


kubectl scale deployment/first-app --replicas=3

docker build -t academind/kub-first-app .
docker push academind/kub-first-app

# Update image
kubectl set image deployment/first-app kub-first-app=academind/kub-first-app
# View current update status
kubectl rollout status deployment/first-app

# Roll back
kubectl rollout undo deployment/first-app
kubectl rollout history deployment/first-app
kubectl rollout history deployment/first-app --revision=3
kubectl rollout undo deployment/first-app --to-revision=1


kubectl apply -f=deployment.yaml
kubectl delete -f=deployment.yaml
```

## Interacting with containers and viewing logs

Most modern applications have some kind of logging mechanism. Our Java REST service, for example, uses slf4j to output logs from the REST controller. The easiest and most simple logging method for containerized applications is just to write to the standard output and standard error streams. Kubernetes supports this out of the box.

Assuming we've sent requests to our new web service using the browser or curl, we should now be able to see some logs. Prior to that, we need to have a Pods name, created automatically during deployment. To get the Pod's name, use the kubectl get pods command. After that, you can show logs of the specified Pod:

```markup
$ kubectl logs rest-example-3660361385-gkzb8 
```

As you can see in the following screenshot, we will get access to a well-known Spring Boot banner coming from a service running in a Pod:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/36cca56d-f361-44a0-b660-781ecf837960.png)

Viewing the log is not the only thing we can do with a specific Pod. Similar to Docker (a Pod is running Docker, actually), we can interact with a container by using the kubectl exec command. For example, to get a shell to the running container:

```markup
$ kubectl exec -it rest-example-3660361385-gkzb8 -- /bin/bash
```

The previous command will attach your shell console into the shell in the running container, where you can interact with it, such as listing the processes, for example, as you can see in the following screenshot:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/16971831-7cbb-41f4-87cc-7787c7754f8c.png)

Having the possibility to view logs and interact with the containers gives you a lot of flexibility to pinpoint potential problems you may have with running Pods. Let's summarize the kubectl commands related to viewing logs and interacting with the Pods in a table:

| **Example command**                                | **Meaning**                                                    |
| -------------------------------------------------- | -------------------------------------------------------------- |
| kubectl logs myPod                                 | Dump pod logs (stdout)                                         |
| kubectl logs myPod -c myContainer                  | Dump pod container logs (stdout, multi-container case)         |
| kubectl logs -f myPod                              | Stream pod logs (stdout)                                       |
| kubectl logs -f myPod -c myContainer               | Stream pod container logs (stdout, multi-container case)       |
| kubectl run -i --tty busybox --image=busybox -- sh | run pod as interactive shell                                   |
| kubectl attach myPod -i                            | Attach to running container                                    |
| kubectl port-forward myPod 8080:8090               | Forward port 8080 of Pod to your to 8090 on your local machine |
| kubectl exec myPod -- ls /                         | run command in existing pod (one container case)               |
| kubectl exec myPod -c myContainer -- ls /          | run command in existing pod (multi-container case)             |
| kubectl top pod POD_NAME --containers              | Show metrics for a given pod and its containers                |

## Scaling manually

Of course, our Java rest-example service keeps its data in memory so it's not stateless, so it may be not the best example for scaling; if another instance is brought to life, it will have its own data. However, it is a Kubernetes service, so we can use it to demonstrate scaling anyway. To scale up our rest-example deployment from one up to three Pods, execute the following kubectl scale command:

```markup
$ kubectl scale deployment rest-example --replicas=3 
```

After a short while, in order to check, execute the following commands, you will see that now three Pods are running in the deployment:

```markup
$ kubectl get deployments
$ kubectl get pods      
```

In the following table, you can see some more examples of kubectl commands related to manual scaling:

| **Example command**                                                     | **Meaning**                                                                |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| kubectl scale deployment rest-example --replicas=3                      | Scale a deployment named rest-example to 3 Pods                            |
| kubectl scale --replicas=3 -f deployment.yaml                           | Scale a resource specified in deployment.yaml file to 3                    |
| kubectl scale deployment rest-example --current-replicas=2 --replicas=3 | If the deployment named rest-example current size is 2, scale it to 3 Pods |
| kubectl scale --replicas=5 deployment/foo deployment/bar                | Scale multiple deployments at one time                                     |

Scaling can be done automatically by Kubernetes, if, for example, the service load increases.

## Autoscaling

With horizontal Pod auto scaling, Kubernetes automatically scales the number of Pods in a deployment or ReplicaSet based on observed CPU utilization. The Kubernetes controller periodically adjusts the number of Pod replicas in a deployment to match the observed average CPU utilization to the target you specified.

The Horizontal Auto Scaler is just another type of resource in Kubernetes, so we can create it as any other resource, using the kubectl commands:

- kubectl get hpa: List autoscalers
- kubectl describe hpa: Get detailed description
- kubectl delete hpa: Delete an autoscaler

Additionally, there is a special kubectl autoscale command for easy creation of a Horizontal Pod Autoscaler. An example could be:

```markup
$ kubectl autoscale deployment rest-example --cpu-percent=50 --min=1 --max=10  
```

The previous command will create an autoscaler for our rest-example deployment, with the target CPU utilization set to 50% and the number of replicas between 1 and 10.

All cluster events are being registered, including those which come from scaling, either manually or automatically. Viewing cluster events can be helpful when monitoring what exactly is being performed on our cluster.

## Viewing cluster events

To view cluster events, type the following command:

```markup
$ kubectl get events  
```

It will present a huge table, with all the events registered on the cluster:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/fe2f17e1-47ef-4d21-85f9-22045ab6cd21.png)

The table will include the changes in the status of nodes, pulling Docker images, events of starting and stopping containers, and so on. It can be very handy to see the picture of the whole cluster.

## Using the Kubernetes dashboard

Kubernetes dashboard is a general purpose, web-based UI for Kubernetes clusters. It allows users to manage applications running in the cluster and troubleshoot them, as well as manage the cluster itself. We can also edit the manifest files of deployment, services, or Pods. The changes will be picked up immediately by Kubernetes, so it gives us the capability to scale down or up the deployment, for example.

If you open the dashboard with the minikube dashboard command, it will open your default browser with a dashboard URL. From here, you can list all the resources on the cluster, such as deployments, services, Pods, and so on. Our dashboard is no longer empty, as you can see in the following screenshot; we have one deployment called rest-example:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/b790112e-df3c-4da5-b9f8-dc79f7d164ac.png)

If you click on its name, you will be taken to the deployment details page, which will show the same information you could get with the kubectl describe deployment command, with a nice UI:

## Cleaning up

If you finish playing with your deployment and services or would like to start from the beginning, you can do some cluster cleaning by removing the deployment or services:

```markup
$ kubectl delete deployment rest-example
$ kubectl delete service rest-example      
```

This code can also be combined in one command, for example:

```markup
$ kubectl delete service,deployment rest-example
```

The kubectl delete supports label selectors and namespaces. Let's see some other examples of the command in a table:

| **Example command**                          | **Meaning**                                          |
| -------------------------------------------- | ---------------------------------------------------- |
| kubectl delete pod,service baz foo           | Delete pods and services with same names baz and foo |
| kubectl delete pods,services -l name=myLabel | Delete pods and services with label name=myLabel     |
| kubectl -n my-ns delete po,svc --all         | Delete all pods and services in namespace my-ns      |

To stop the minikube cluster, issue simply:

```markup
$ minikube stop 
```

If you would like to delete the current minikube cluster, you can issue the following command to do it:

```markup
$ minikube delete 
```

## Authentication

By default, the Kubernetes API server serves HTTP requests on two ports:

- **Localhost**, **unsecured port**: By default, the IP address is `localhost` and a port number is `8080`. There is no TLS communication, all requests on this port bypasses authentication and authorization plugins. This is intended for testing and bootstrap, and for other components of the master node. This is also used to other Kubernetes components such as scheduler or controller-manager to execute API calls. You can change the port number with the `--insecure-port` switch, and the default IP by using the `--insecure-bind-address` command-line switch.
- **Secure port**: The default port number is `6443` (it can be changed with the `--secure-port` switch), usually it's `443` on Cloud providers. It uses TLS communication. A certificate can be set with a `--tls-cert-file` switch. A private SSL key can be provided with a `--tls-private-key-file` switch. All requests coming through this port will be handled by authentication and authorization modules and admission control modules. You should use the secure port whenever possible. By having your API clients verify the TLS certificate presented by the `api-server`, they can verify that the connection is both encrypted and not susceptible to man-in-the-middle attacks. You should also be running the api-server where the insecure port is only accessible to localhost, so that connections that come across the network use `HTTP's`.
- With minikube, to access the API server directly, you'll need to use the custom SSL certs that have been generated by minikube. The client certificate and key are typically stored in `~/.minikube/apiserver.crt` and `~/.minikube/apiserver.key`. You'll have to load them into your HTTP'S client when you make HTTP requests. If you're using curl use the--cert and the --key options to use the cert and key file.

## Role-based access control (RBAC)

The **Role-Based Access Control** (**RBAC**), policy implementation is deeply integrated into Kubernetes. In fact, Kubernetes uses it internally for the system components, to grant the permissions necessary for them to function. RBAC is 100% API driven, roles and bindings are API resources that an administrator can write and create on the cluster such as other resources such as Pods, deployments, or services. Enabling RBAC mode is as easy as passing a flag to kube-apiserver:

```markup
--authorization-mode=RBAC 
```

This mode allows you to create and store policies using the Kubernetes API. In the RBAC API, a set of permission is represented by the concept of role. There is a distinction between namespace roles, represented by a Role resource, and a whole cluster role, represented by a ClusterRole resource. A ClusterRole can define the same all permissions a Role can define, but also some cluster-related permission, such as managing cluster nodes or modifying resources across all available namespaces. Note that once RBAC is enabled, every aspect of the API is disallowed access.

This is an example of role that gives the whole set of available permissions to all operations on all resources:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: cluster-writer
rules:
  - apiGroups: ["*"]
    resources: ["*"]
    verbs: ["*"]
    nonResourceURLs: ["*"]
```

The Role is a resource, as you remember from [Chapter 8](https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/8), *Using Kubernetes with Java*, to create resource using the file, you execute the kubectl create command, for example:

```markup
$ kubectl create -f cluster-writer.yml
```

A Role and ClusterRole defines the set of permissions, but does not assign them to users or groups directly. There is another resource for that in Kubernetes API, which is RoleBinding or ClusterRoleBinding. They bind Role or ClusterRole to the specific subject, which can be user, group, or service user. To bind the Role or ClusterRole, you will need to execute the kubectl create rolebinding command. Take a look at the following examples. To grant the adminClusterRole to a user named john in the namespace myApp:

```markup
$ kubectl create rolebinding john-admin-binding \
--clusterrole=admin --user=john --namespace=myApp 
```

The next one will grant the cluster-admin ClusterRole to a user named admin across the entire cluster:

```markup
$ kubectl create clusterrolebinding admin-cluster-admin-binding \
--clusterrole=cluster-admin --user=admin  
```

The equivalent YAML file to use with kubectl create -f will be as follows:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: admin-cluster-admin-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name cluster-admin
subjects:
- kind: User
  name: admin
```

# Volumes

## emptyDir

deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: story-deployment
spec: 
  replicas: 1
  selector:
    matchLabels:
      app: story
  template:
    metadata:
      labels:
        app: story
    spec:
      containers:
        - name: story
          image: academind/kub-data-demo:1
          volumeMounts:
            - mountPath: /app/story
              name: story-volume
      volumes:
        - name: story-volume
          emptyDir: {}
```

service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: story-service
spec:
  selector: 
    app: story
  type: LoadBalancer
  ports:
    - protocol: "TCP"
      port: 80
      targetPort: 3000
```

```bash
docker build -t academind/kub-data-demo:1 .
docker push academind/kub-data-demo:1
kubectl apply -f=service.yaml -f=deployment.yaml
minikube service story-service
```

## hostPath

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: story-deployment
spec: 
  replicas: 2
  selector:
    matchLabels:
      app: story
  template:
    metadata:
      labels:
        app: story
    spec:
      containers:
        - name: story
          image: academind/kub-data-demo:1
          volumeMounts:
            - mountPath: /app/story
              name: story-volume
      volumes:
        - name: story-volume
          hostPath:
            path: /data
            type: DirectoryOrCreate
```

## Defining a Persistent Volume

host-pv

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: host-pv
spec:
  capacity: 
    storage: 1Gi
  volumeMode: Filesystem
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data
    type: DirectoryOrCreate
```

host-pvc

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: host-pvc
spec:
  volumeName: host-pv
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  resources:
    requests: 
      storage: 1Gi
```

ReadWriteOnce means that this volume can be mounted as a read-write volume by a single node. So by multiple pods, but they all have to be on the same node.

ReadOnlyMany means that it's read-only but it can be claimed by multiple nodes. So multiple pods on different nodes can claim this same persistent volume. And for example, for the host path type this is simply not in available option because host path by definition is defined on one node and they offer another node and another pod running on another node won't be able to claim it.

ReadWriteMany is therefore also not available because that's the same like ReadOnlyMany, just with the difference that we're talking about read-write access.

deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: story-deployment
spec: 
  replicas: 2
  selector:
    matchLabels:
      app: story
  template:
    metadata:
      labels:
        app: story
    spec:
      containers:
        - name: story
          image: academind/kub-data-demo:2
          env:
            - name: STORY_FOLDER
              # value: 'story'
              valueFrom: 
                configMapKeyRef:
                  name: data-store-env
                  key: folder
          volumeMounts:
            - mountPath: /app/story
              name: story-volume
      volumes:
        - name: story-volume
          persistentVolumeClaim:
            claimName: host-pvc
```

# ConfigMap

environment

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: data-store-env
data:
  folder: 'story'
  # key: value..
```



# Example

## React

[Online Courses - Learn Anything, On Your Schedule | Udemy](https://www.udemy.com/course/docker-kubernetes-the-practical-guide/learn/lecture/22627949#overview)

```docker
FROM node:14-alpine as builder

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

RUN npm run build

FROM nginx:1.19-alpine

COPY --from=builder /app/build /usr/share/nginx/html

COPY conf/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD [ "nginx", "-g", "daemon off;" ]
```
