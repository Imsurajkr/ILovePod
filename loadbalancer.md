## Let's discuss load balncer 

Its very same as nodeport 

``` 
kubectl create pods <podName> --type LoadBalancer --port 0
# Instead of NodePort you have to use Load balancer and you can access the service running inside the pods
```
Let's discuss what's difference in Load balancer <br />


