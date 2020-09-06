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
kubectl run <podName> --image=ngnix --port 80 --dry-run -o yaml
# use io stream to save the data inside a file 
kubectl run <podName> --image=ngnix --port 80 --dry-run -o yaml > bestway.yml
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
  - image: ngnix # image name from docker hub
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
  - image: ngnix # image name from docker hub
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
  selector: # is used for matching the labels of pods 
    run: suraj1 # we have to change the label as the pod file so that we can identify the pods in which its running.
  type: NodePort
status:
  loadBalancer: {}
```