## K8S 

An open-source project that automates the process of deploying and managing multi-container applications at scale. Kubernetes came into existence in mid-2014 and in a short span of time grown as an open-source community with engineers from Google, Red Hat, and many other companies contributing to the project.


### Major components in Kubernetes 

Now let's checkout some cool important components of k8s.

A project managed by Cloud Native Computing Foundation.

- kubeApi Server
- Kube Scheduler
   Kube scheduler schedules the minion.
- Node Controller 
   It takes the responsibility to check the minion health it can add a minion to the kubernetes cluster 
- Replication Controller 
   Maintaining the container count 

- Control Pane 
   The KubeApi server, Replication controller these components can be called control pane component.


** How Kubernetes will remember the status of cluster **
If the applications are failing and every detailed information saved in etcd folder.
Every single thing is saved in `ETCD` No sql Database
[Link](url) and ![Image](src)

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Networking in k8s

** k8s doesn't use docker bridge and doesn't use docker networking so how k8s can communicate ?**
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
** So you can image a tool to manage container can be run inside a container and inside the container it will manage your container and interact with the docker engine **
Inside a container another container is running which triggers a kubernetes cluster 

Command you require to run a container engine of minikube you can exectue the following command 

```
minikube start --driver=<driver_name> # here the driver name you have to specify vmbox or docker.
# If you want to use docker so you can run it with 
minikube start --driver=docker
# To take the shell of the docker container 
docker exec -it minikube bash 
```
No Minikube is running in your local system and you can access the nodes which are running in your local environment by 
```
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
If you want to connect to any file on the remote location you can always use the command 

```
kubectl get nodes --kubeconfig <fileName>.conf
# In windows you have to specify the complete path of the file
```


### Pods 
The containers are encapsulated known as pods. We can say like pod is a wrapper over container 
Short story which might help us to understand better :-
Think of a big shop which produces a good quality of clothes such as shirts,pants a local vendor purchase those products and sell it on local market . Good reputed firms such as Zara ,US polo also purchase those products a little touch/change is added and tag is attached now its a Quality product 
We can think Pod as a zara product which makes it more cool and awesome.
``` 
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

A sample Pod fiapiVersion: v1
```
kind: Service 
metadata: 
  name: myservice1

spec:
  ports:
   - name: mysvcport # Optional field
     port: 1122 # this is must and is the port of service IP 
     targetPort: 80 # this must match the port number of POD 
     protocol: TCP  # optional Field
  selector:
   x: hello # this label must be same as pod label where we want to forwad the service 
  type: NodePort
```




### Support or Contact

Have any doubt regariding the docs please Mail [infosurajkumard@gmail.com](#).
New updates Coming soon
Go to the next page[here](./new-page.html)