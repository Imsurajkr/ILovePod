## Let's Explore More 

## Now its time to imagine 

I am person on the local system and my k8s cluster is working and somehow a pod goes down so whats going to happen now.
It will not be scheduled by any component in the cluster.
We wrote a yaml file and passed the request to kubeSchedular and it pass to pod and kubelet and kubelet will pass the information to Api server and written to `etcd` if the pod goes to down it sends the data to  node controller and it will conect to Api sever and save the data to `etcd`.
<br />

## Replication Controller [ RC ]

So actually in real world no one deploying a pod. It is not recommended directly manage the pods by client<br />

Replication Controller is the one which manage the count of pods and you have to manage the replication controller and replecation controller will manage that for you.<br />

We also call it RC in short.

Lets create a replecation controller in the pod and check whats happen when the pods goes down.<br />

It's a way to automanage a pod.

We have to write a yaml file for the replication controller
 ```bash 
vim replicas.yml
# Inside the yml file we can mention the details.
apiVersion: v1
kind: ReplicationController
metadata:
  name: myrcsuraj
  labels: 
    app: web1 # This is the label of RC not label of pods

spec:
  replicas: 1 # Always on pod will be present even in case of current worker node failure 
  template:
   metadata:
    name: mypod11 # This will be the podname that will be created by RC.
    labels:
     x: hellosuraj
   spec:
    containers:
     - name: suraj111
       image: nginx
       ports:
        - containerPort: 80
  # You have to give the details about the pod in case if the pods goes down then the repication have to access the template 
  # so we will also mention the template.
  # In case if pod goes down it will re-create
```

# There is no auto generated method of replication controller 
# we can take help of pod file and get the data from the pod file

```bash
kubectl explain replectioncontroller.spec
# Let's run the file by  
kubectl create -f rc.yml
kubectl get rc # will display the contents of Replication Controller
kubectl get replicationcontrollers # will display the replication controllers
kubectl get po 
# and you can get the pod visible there
kubectl get pods --show-label
# to check the labels
# -----------------------------
# Even if you delete the pod Replication controller 
# It will auto create a new pod for you
kubectl delete pods <podname>
# you can see another pod is up and running in front of you
kubectl get po
# Expose the service by 
kubectl expose replicationcontroller <podname> --type NodePort --port 80 
# Get the service list by 
kubectl get svc
# get the port number and check the status
```



