## 

## Important note for Docker Desktop and Driver Users

[Online Courses - Learn Anything, On Your Schedule | Udemy](https://www.udemy.com/course/kubernetes-microservices/learn/lecture/23102800#overview)

If you're using the Docker Driver, you will need to issue a command to access your service. Using "minikube ip" won't work!

If you are using the Kubernetes that ships with Docker, then you just use "localhost" or "127.0.0.1" as the address. Don't forget the port number of your NodePort. so "127.0.0.1:30080" should work for the webapp.

For some users, localhost doesn't work and you will need the following command:

kubectl port-forward webapp 30080:80

You now need to leave this process running - you will need to continue your work in another terminal/command prompt.

If you are using the Docker Driver, then you will need to open up a terminal/command prompt window/powershell and run the following command:

minikube service fleetman-webapp

This will return some output like this:

![image.png](https://codahosted.io/docs/0-Ai5LKD5z/blobs/bl-vhHzpLcZUq/d607139f5c265aa802ed9157fb6c67afd44732b83269ba281820cf90e5a4cd0f46a53f8ecb73c7fcc7b6a0a06b9517f271521abc3c3a02b051aeb1760dcc1a796021eaf354dcc66c1fea4a27b378e71dd1c826c34a72f6eb9769960e467bcc552643a3f2)

This should automatically open the application in a browser tab. If not, the address that you need to access in your browser is given at the bottom of the output (after the "starting tunnel" announcement) - in this example, it will be "http://127.0.0.1:56064". The port number (56064 in this case) is generated randomly so make a careful note of it.

You now need to leave this process running - you will need to continue your work in another terminal/command prompt.

[docker - Can&#39;t access minikube service using NodePort from host on Mac - Stack Overflow](https://stackoverflow.com/questions/63600378/cant-access-minikube-service-using-nodeport-from-host-on-mac)

You are mostly facing [this issue](https://github.com/kubernetes/minikube/issues/7344) when you use minikube ip which returns 127.0.0.1. It should work if you use internal ip from kubectl get node -o wide instead of 127.0.0.1.

A much easier approach from the official reference [docs](https://kubernetes.io/docs/tutorials/hello-minikube/) is you can get the url using minikube service web-test --url and use it in browser or if you use minikube service web-test it will open the url in browser directly

Admin@LAPTOP-QO8E8EAL /cygdrive/d/devops-practices/k8s/Chapter 6 Services  
$ minikube service fleetman-webapp --url  

```bash
Admin@LAPTOP-QO8E8EAL /cygdrive/d/devops-practices/k8s/Chapter 6 Services
$ minikube service fleetman-webapp --url
* Starting tunnel for service fleetman-webapp.
|-----------|-----------------|-------------|------------------------|
| NAMESPACE |      NAME       | TARGET PORT |          URL           |
|-----------|-----------------|-------------|------------------------|
| default   | fleetman-webapp |             | <http://127.0.0.1:57237> |
|-----------|-----------------|-------------|------------------------|
<http://127.0.0.1:57237>
! Because you are using a Docker driver on windows, the terminal needs to be open to run it.
```

---

## Kubernetes Hands-On - Deploy Microservices to the AWS Cloud

### 1.2 K8S Command Reference.pdf.pdf

Here's a run down of all (ok, most of) of the commands used in the

Kubernetes Microservices course

Minikube

```bash
minikube start
```

starts minikube. If this hangs (wait 15 minutes), then see the video in section 3

that addresses common problems

```bash
minikube stop
```

(Section 3): stops the minikube virtual machine. This may be necessary to do if

you have an error when starting

```bash
minikube delete
```

(Section 3): do this to completely wipe away the minikube image. Useful if

minikube is refusing to start and all else fails. Also you can delete all files in

/.minikube and /.kube

```bash
minikube env
```

(Section 4): find out the required environment variables to connect to

the docker daemon running in minikube.

```bash
minikube ip
```

(Section 4 or 5): find out the ip address of minikube. Needed for browser access.

Kubectl

```bash
kubectl get all
```

(Section 5): list all objects that you’ve created. Pods at first, later,

ReplicaSets, Deployments and Services

```bash
kubectl apply –f
```

(Section 5): either creates or updates resources depending on the

contents of the yaml file

```bash
kubectl apply –f .
```

(Section 7): apply all yaml files found in the current directory

```bash
kubectl describe pod <pod name>
```

(Section 5): gives full information about the specified pod

```bash
kubectl exec –it <pod name> <command>
```

(Section 5): execute the specified command in the pod’s container.

Doesn’t work well in Cygwin.

```bash
kubectl get (pod | po | service | svc | rs | replicaset | deployment | deploy)
```

(Section 6): get all pods or services. Later in the course, replicasets and

deployments.

```bash
kubectl get po --show-labels
```

(Section 6): get all pods and their labels

```bash
kubectl get po --show-labels -l {name}={value}
```

(Section 6): get all pods matching the specified name:value pair

```bash
kubectl delete po
```

(Section 8): delete the named pod. Can also delete svc, rs, deploy

```bash
kubectl delete po --all
```

(Section 8): delete all pods (also svc, rs, deploy)

Deployment Management

```bash
kubectl rollout status deploy <name of deployment>
```

(Section 9): get the status of the named deployment

```bash
kubectl rollout history deploy <name of deployment>
```

(Section 9): get the previous versions of the deployment

```bash
kubectl rollout undo deploy <name of deployment>
```

(Section 9): go back one version in the deployment. Also optionally `--to-revision=<revision number>`

We recommend this is used only in stressful emergency situations! Your

YAML will now be out of date with the live deployment!

```bash
kubectl logs <pod_name>  
kubectl logs -f <pod_name>
```

## WARNING - possible resource problems! minikube

In this section, we're going to be deploying a lot more resources to your cluster.

I've realised since recording that on some systems, there won't be enough resources (RAM) in minikube to manage the full load. For some reason, I got away with it on the videos (although in a later section, when we use Mongo, I did have to expand the RAM).

I recommend before starting this section that you set up minikube with plenty of RAM. To do this:

1. Stop minikube with "minikube stop"
2. Delete your existing minikube with "minikube delete"
3. Remove any config files with "rm -rf ~/.kube" and "rm -rf ~/.minikube". Or, delete these folders using your file explorer. The folders are stored in your home directory, which under windows will be "c:\Users\<your username>"
4. Now restart minikube with "minikube start --memory 4096".

This will allocate 4Gb of RAM to minikube (I'm assuming you have enough host ram to support this) and should give a much more comfortable experience in the next few sections.

Do ask on the QA page if you have any problems here.

For those using the Kubernetes supplied with Docker Desktop:

Launch Docker Desktop, go to Preferences-->Resources, increase the memory to 4GB (or more) then click Apply and Restart.a
