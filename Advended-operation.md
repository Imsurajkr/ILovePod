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

