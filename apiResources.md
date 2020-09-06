## Let's explore more!!!

## What is Pod and Service 

If someone asks what is a pod or service.<br />

## Api-Resources 

The answer is pretty simple is API-Responses.
The Pod, Service or nodes these all are the api-resources last month its 68
```bash
kubectl api-resources 
# You can check the api resources 
kubectl api-resources | wc -l 
# you can check the latest count of api-resources
kubectl api-resources | less
# to check about the resources  
```
# Now its time to imagine 

I am person on the local system and my kubernetese cluster is working and somehow a minion goes down so whats going to happen with now.
It will not be scheduled by any component in the cluster.
We wrote a markup file and we created a file and passed the request to kubeSchedular pass to pod and kubelet and kubelet passed the information to Api server and writtent to `etcd` if the pod goes to down it sends to Node controller and it will conect to Api sever and save the data to `etcd`.
<br />


