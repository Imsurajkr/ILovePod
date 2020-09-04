## Hola Mundo  
### Welcome to My kubernetes Jouney

I am a DevOps engineer at Fiftyfivetech.<br />
I will share deep dive knowledge of kubernetes which I learned from various sources.<br />
I am assuming the reader have some knowledge of Docker.<br />
Familiar with Contanirezed technology and Images.<br />
Bonus :- We will automate the Azure kubernetes service with terraform.<br />

### Let's start contanirezed app deployment in real world.

What happens in the real time scenerio is a developer writes a code attach a docker file.<br />
Build a image and deploy it to anywhere where the docker is installed such as
1. DE[ Docker Engine ] running in a server. 
2. DE[ Docker Engine ] running in a Pc.
3. DE[ Docker Engine ] running in a cloud.

Things which are needed to be taken care of 
1. Downtime 
2. Security
3. Upgrade 
4. Portability 

An open-source project that automates the process of deploying and managing multi-container applications at scale. Kubernetes came into existence in mid-2014 and in a short span of time grown as an open-source community with engineers from Google, Red Hat, and many other companies contributing to the project.

A project managed by Cloud Native Computing Foundation.

### Major components in Kubernetes 

Now let's checkout some cool important components of k8s.

- kubeApi Server
- Kube Scheduler
   Kube scheduler schedules the minion.
- Node Controller 
   It takes the responsibility to check the minion health it can add a minion to the kubernetes cluster 
- Replication Controller 
   Maintaining the container count 

- Control Pane 
   The KubeApi server, Replication controller , Master , Minion / Node, etcd these components are togther called control pane.


** How Kubernetes will remember the status of cluster **
If the applications are failing and every detailed information saved in etcd folder.
Every single thing is saved in `ETCD` No sql Database
[Link](url) and ![Image](./assets/image/docker.jpg)

### Networking in k8s

**k8s doesn't use docker bridge and doesn't use docker networking so how k8s can communicate ?**
    Yes, That's a really good point in k8s we cannot use the network resources shared by docker such 
    as docker0.

We will create a bridge <yourNameBridgeNetwork>. Which will be connection to all the resouorces in the minion.
Create container any where the ip will be assigned from <yourNameBridgeNetwork> only and master will also use that bridge only

In present market there are so many networks which can help us with such features 
1. Flannel 
2. Callico 
3. Cisco ASI 
4. Weave-net 
5. calico 

You can use any one to create this bridge.

Kube-Proxy will maintain the communication between the containers accross the worker nodes.

### Types of Installation.
1. Single node k8s cluster 

If you have minikube and configure custer than VM ware , Virtual box.
The coolest part is you can setup a container and that container will setup the minikube cluster inside the container 
**So you can image a tool to manage container can be run inside a container and inside the container it will manage your container and interact with the docker engine**
Inside a container another container is running which triggers a kubernetes cluster 

Command you require to run a container engine of minikube you can exectue the following command 

```bash
minikube start --driver=<driver_name> # here the driver name you have to specify vmbox or docker.
# If you want to use docker so you can run it with 
minikube start --driver=docker
# To take the shell of the docker container 
docker exec -it minikube bash 
```
No Minikube is running in your local system and you can access the nodes which are running in your local environment by 

```bash
kubectl get nodes # The output will look like this 
# NAME       STATUS   ROLES    AGE   VERSION
# minikube   Ready    master   21h   v1.18.3
```
2. Multi Node k8s cluster
    2.a) 1 Master N-Minion 
    2.b) Multi Master N-minion

3. Kubernetes as a service / Cloud provider 
    1. EKS
    2. AKS
\n If you want to connect to any file on the remote location you can always use the command 

```bash
kubectl get nodes --kubeconfig <fileName>.conf
# In windows you have to specify the complete path of the file
```


### Pods 
The containers are encapsulated known as pods. We can say like pod is a wrapper over container 
Short story which might help us to understand better :- \n
Think of a big shop which produces a good quality of clothes such as shirts,pants a local vendor purchase those products and sell it on local market . Good reputed firms such as Zara ,US polo also purchase those products a little touch/change is added and tag is attached now its a Quality product 
We can think Pod as a zara product which makes it more cool and awesome.
``` bash
kubectl run nginx -image nginx
kubectl get pods 
# To check the network assigned to the pods 
kubectl get pods -o wide 
# To get more data about the pods 
kubectl describe pod < Pod Name >
```
### How to create pods in Kubernetes.
There are two methods to create Pods in Kubernetes 
1. YAML 
2. JSON

Basically we have to remember 4 major points to before creating a Yaml file 
1. apiVersion 
2. kind
3. Metadata
4. specification 


**A sample Pod file** 
```bash
apiVersion: v1
kind: Pod #Here P is Capital
metadata: # Some information about the pod  
  name: <YourName>pod # This is my pod name like container name in docker containers
spec:
  containers: # About my docker images and Containers info 
    - image: nginx # This is the data of the image which will be pulled from the hub.docker.io
      name: <YourName>pod # The Name if the container will be suraj pod ** You don't need unique name 
      ports:
        - containerPort: 80 # same as expose port in docker file
```

**The command to launch the pods**
```bash 
kubectl create -f <podFileName>.yml
```

### Working in Kubernetes 

The basic flow in which a pod is deployed in kubernetes.

1. Create a YML or JSON file run from kubectl command.
2. It goes to kubeApiServer. It takes the request to scheduler.
3. Kube-scheduler will send this to kubelet. 
4. kubelet takes request over minions.
5. Kubelet will write the information and inform Kubeapi server that its running pod scheduled. 
6. KubeApiServer will write this information to database in `etcd` .
7. etcd stores the data about how much resources are used in pod and all information regarding pod.

## Best practice to write a pod file.

1. Create a manual pod file which is by opening editor and  write the yaml. 

Important terms to remember is **always prefer ports keywords and container port**
Commands to run the kubernetes cluster 

```bash 
kubectl get pods # To get the pods details 
kubectl create -f <podfile>.yml # to create the pod from yaml file 
# You can also update the pod but only certain fields can be changed not each fields can be updated
kubectl apply <podName>
# To delete the Pod 
kubectl delete pod <podName>
# To delete all the Pods 
kubectl delete pod --all
# To recreate 
kubectl create -f <podfile>.yml
```
**How we can check detailed information about pods**
```bash 
kubectl describe pods <podName>
```
#### How we can write a pod file without remembring it ??

```bash 
# We can get the details about the Pod file using 
kubectl explain pods | less # it will take you to page 1 and display all the commands 
# To get the details about the specific componets of pod file.
kubectl explain pods.apiversion | less
# To get the details about the Kind component of pod file 
kubectl explain pods.kind | less
# To get the  details about the spec component of pod file 
kubectl explain pods.spec | less
# To get specific components details .
kubeclt explain pods.spec.containers.ports | less
```
## Another best method to Auto generate pod file.

**We can use kubectl command to generate the Pod file** 
```bash 
kubectl run <podName> --image=nginx --port 80 --dry-run -o yaml 
# the --dry-run here will not create a pod but simply checks the syntax 
# Redirect it to some file so we can use it later 
kubectl run <podName> --image=nginx --restart Never --dry-run -o yaml > wow.yaml
# It will create a file with the name wow.yaml . 
# If you want to generate the json you can change the -o json   
# --restart is a policy used 
```

**Key takeaways till here**
1. Manage pod communication across worker node . 
2. Kubernetes client - kubectl .
3. Path token config file in kubernetes is `/etc/kubernetes/admin.conf` .
4. `Kubectl get ns` It willl give you list of all the namespacecs. 
5. To check the labels in the pods kubectl get pods --show-labels.
6. To take help about the apiVersion `kubectl explain <resourceType>`    

Some commands to interact with minikube 
<script src="https://gist.github.com/Imsurajkr/2540dd6ca5597686981b6397383bc760.js"></script>

## So Pod can be created by 5 ways 
1. YAML
2. Command Line 
3. JSON 
4. API
5. Programming 

## Here arises a question how to access your application running in a pod ?

Let's Assume Ankita, Mansi, Anurag are working in a project and they have to setup a cluster of kubernetes 
<script src="https://gist.github.com/nisrulz/11c0d63428b108f10c83.js"></script>


<script src="https://gist.github.com/Imsurajkr/6639240790c82866af1f644589542c46.js"></script>

### Support or Contact

Have any doubt regariding the docs please Mail [surajkumar.devp@gmail.com](#).
New updates Coming soon
Go to the next page[here](./new-page.html)