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

### Support or Contact

Have any doubt regariding the docs please Mail [infosurajkumard@gmail.com](#).
New updates Coming soon
Go to the next page[here](./new-page.html)