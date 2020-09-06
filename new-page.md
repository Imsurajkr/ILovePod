## Hello World

Pod :- So containers are encapsulated inside pod and pod is the one that having container inside the pod and to access a application running we have to create a service inside the pod.

Service we have discussed 
1. cluster ip 
2. Nodeport 
3. Load balancer 
4. External Name
oouth of these Nodeport and load balancer are the one if you want to access the servcie outside of your pod

The range of Nodepod is 30,000-32,767 you will get the pod number. So that people can access the pod .

## Best practice or way to write pod and servcie file.

The best practice to write a pod file 
```bash 
kubectl run <podName> --image=nginx --port 80 --dry-run -o yaml
# use io stream to save the data inside a file 
kubectl run <podName> --image=nginx --port 80 --dry-run -o yaml > bestway.yml
```
### Its always a good practice to mention the terms you used in you Pod file
I have mentioned some important terms in this file.
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels: # label or pod 
    run: suraj1
  name: suraj1 # name of pod
spec:
  containers:
  - image: nginx # image name from docker hub
    name: suraj1 # name of container 
    ports: # default port of docker image with application 
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
Whenever we create a pod always create service as well.
```bash
kubectl create service nodeport <nameOfService> --tcp 80 --dry-run -o yaml
# I can store in a new file but in real time scenario if we have 10 pod and 10 service its difficult to manage so <br />
# We will save the pod file and service file in one file
kubectl create service nodeport <nameOfService> --tcp 80 --dry-run -o yaml >>bestway.yml
# The >> will append the file and it will update in your.
```
### After this step 
Your file might look like this
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels: # label or pod 
    run: suraj1
  name: suraj1 # name of pod
spec:
  containers:
  - image: nginx # image name from docker hub
    name: suraj1 # name of container 
    ports: # default port of docker image with application 
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
--- # put three - in the beginning of service file. This is the best way to pod and service.
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels: # label of service
    app: surajbest1
  name: surajbest1 # name of service
spec:
  ports:
  - name: "80"
    port: 80 # service port that is going yo bind with service IP and this port can be anything it is not dependent on cont port
    protocol: TCP
    targetPort: 80 # target port where servcie will forward all its traffic that is same as pod port.
  selector: # There's no chance of mistake here we have to keep it same as it was in pod file 
    run: suraj1 # we have to change the label as the pod file so that we can identify the pods in which its running.
  type: NodePort
status:
  loadBalancer: {}
```
## To delete all the previous pods and service and launch a pod with attached service 

```bash 
kubectl delete pods,svc --all
# To create a pod as well as service file you can execute.
kubectl create -f bestwaypod.yml
# Boom now pod ans service both are created and you can access the pod and service both by 
kubectl get pod 
# you can also check the pod by 
kubectl get po
# you can check the ip by 
kubectl get po -o wide
# to watch the container running 
kubectl get po -w 
# here -w is the argument attached to watch the pod 
# to get nodes information
kubectl get nodes
# to get the service detail we can use the
kubectl get services
# you can also check it by 
kubectl get svc
# check the port that node port attached  
# The output of the file looks like this 
# NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
# kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        3d
# surajbest1   NodePort    10.109.91.251   <none>        80:30921/TCP   2m9s
# Here in surajbest1 30921 is the port assigned to me to access the service outside the nodes
# You have to use either master Ip or Minion ip 
kubectl get po -o wide
```
## A small trick i learned .

Just simple steps to follow and access you service running inside a pod.

```bash 
# Delete the previos service by 
kubectl delete svc <serviceName>
# Don't delete the pod 
# you can chcek the label by
kubectl get po <podName> --show-labels 
kubectl expose pods <podName> --type NodePort --port 80 
kubectl get svc 
# you pod is up and runing and check the service it will automatically be created for you
# We don't have to worry about the label of the pod it will automatically take that label and create it for you 

```
## Learn More cool Stuff
### here's a pro tip which i learned 

## From a running pod or service how we can get a json or yml file 
```bash
kubectl get svc <serviceName> -export -o yaml
# to get the details of pod 
kubectl get pod <PodName> --export -o yaml
# if you want to save save it somewhere by 
kubectl get pod <PodName> --export -o yaml > details.yml
```

## Thats all for Kubernetes pod and service

