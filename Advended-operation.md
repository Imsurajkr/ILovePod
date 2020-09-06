## Some Advanced operations

### Lets revise Docker a little bit

In docker when we pull a image from docker registry.
```bash 
docker run -itd --name test1 -p 9091:80 dockerashu/ckad:v2
docker ps 
# you can check your application at location localhost:9091
docker exec -it test1 bash
# Now you are inside the container 
# now you can see the green color written inside the env file 
# Now how can you change color is very simple by using the envvironment varibale
docker run -itd --name test2 -p 9092:80 -e color=yello
dockerashu/ckad:v2
# now you can check on your local host the color is changed 
```

## Now the Question arises here is to how to write these environment configuration in Pod files.<br /> 

```bash 
vim podwithEnv.yml
# add these inside you can also configure vim wiht 
# Esc key and typing :set shiftwidth=1
apiVersion: v1
kind: Pod
metadata
 name: surajpodenv1
 labels:
  x: helloenv
spec
 containers:
  - name: suraj111
    image: dockerashu/ckad:v2
    ports:
     - containerPort: 80
    env: # this is for replacing the ENV variable in POD
     - name: color # This must be the same vaiable that we defined under the docker keyword variable 
       value: yello # you have to remember the environment values as written in the container
```
Expose the pod to check if its running 
```bash 
kubectl get svc 
# To get the pod 
kubectl expose pods <podName> --type LoadBancler/NodePort --port 80 
# here select one which is either NodePort or LoadBalncer
```
## Lets connect some things with docker 

create one Dockerfile, Build the image and push it to the docker hub registry.
```bash 
vim Dockerfile
>FROM alpine 
>MAINTAINER suraj
>ENTRYPOINT ping fb.com
# Add these line inside the Dockerfile 
# Now build the image with 
docker build -t imsurajkr/alpine:entry .
docker run -it --rm imsurajkr/apine:entry
# Now if i want to change the entry point int Images.
docker run -it --rm imsurajkr/apine:entry cal
# it is not going to replaced the entrypoint so 
# What if I have to replace the ping with cal  without replacing docker entrypoint
# We have to replace it in the parent process and we dont want to write another Dockerfile.
# There is a keyword which is --entrypoint cal
docker run -it --rm imsurajkr/apine:entry 
# just push the image to the docker repository 
docker login -u <username> # Mine is imsurajke
# type the password and you will be logged in 
# Now we want the same to be done in pod
vim podwithentryvalue.yml
# ---------- Edit in YAML  ----------
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    x: hello
  name: pod1
spec:
  containers:
  - image: imsurajkr/alpine:entry
    name: pod1
    command: ["/bin/sh","-c","cal"] # command can replace entrypoint parent process in docker
# start with the shell of container.
#------------------------------------------

# Now run the pod with 
kubectl run -f podwithentryvalue.yml
# get pods details with 
kubectl get po
# to restart a pod
kubectl replace -f podwithentryvalue.yml --force
# The pod might gets into crashLoopBackOff position and the reason is 
# Pod is having something which is not going to run forever its going to be created and deleted
kubectl logs pod1
# You can see the cal output 
# What we can do is change the command and run it again command: ["/bin/sh","-c","ping 8.8.8.8"]
kubectl replace -f podwithentryvalue.yml --force
# Now your pod is in running state 
# If you want to see that running condition 
kubectl logs -f pod1
```
## Awesome and cool methods 

# Pod Advanced 

We are a client working on our local system and having a k8s cluster somewhere is the cloud datacenter. My pod is running somewhere in pod2 
and using kubectl command I am communicating with kubeApiServer. 
**Now imagine we are having the access of the pod which is running inside some minion and we can communicate with a program such as Docker engine and get the logs and we can also have the access of the shell of the pod**
Which is running inside the minion 
```bash
kubectl exec -it pod1 sh 
# you can check the parent process by ps -e 
# execute other commands such as 
ping fb.com
# we can check the route table 
route -n
# Its very very simple.
```
## Now thats what you we can say cool !!!



