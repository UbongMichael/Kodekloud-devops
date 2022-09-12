We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.



Create a pod named volume-share-datacenter.

For the first container, use image ubuntu with latest tag only and remember to mention the tag i.e ubuntu:latest, container should be named as volume-container-datacenter-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/ecommerce.

For the second container, use image ubuntu with the latest tag only and remember to mention the tag i.e ubuntu:latest, container should be named as volume-container-datacenter-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/games.

Volume name should be volume-share of type emptyDir.

After creating the pod, exec into the first container i.e volume-container-datacenter-1, and just for testing create a file ecommerce.txt with any content under the mounted path of first container i.e /tmp/ecommerce.

The file ecommerce.txt should be present under the mounted path /tmp/games on the second container volume-container-datacenter-2 as well, since they are using a shared volume.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


Solution
========

verify if any pod is running
```
kubectl get pods
```
```
vi /tmp/volume.yaml # insert the below yaml file
```
```
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-datacenter
  labels:
    name: app
spec:
  volumes:
    - name: volume-share
      emptyDir: {}
  containers:
    - name: volume-container-datacenter-1
      image: ubuntu:latest
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce
    - name: volume-container-datacenter-2
      image: ubuntu:latest
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/games
```

Create the pods by running the following command
```
kubectl create -f /tmp/volume.yaml
```
```
kubectl get pods
```
```
kubectl get pods -o wide
```
Exec ito the containers
```
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-1 --/bin/bash
```

Write any content to the path specified
```
echo "Welcome on Board, Young Devops Engineer!" > /tmp/ecommerce/ecommerce.txt
```

Check if there is a content
```
cat tmp/ecommerce/ecommerce.txt
```

```
exit
```

Verify the content i the second container
```
kubectl exec volume-share-datacenter -c volume-container-datacenter-2 -- ls /tmp/games 
```
