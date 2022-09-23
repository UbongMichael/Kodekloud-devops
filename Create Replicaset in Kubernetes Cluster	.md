The Nautilus DevOps team is going to deploy some applications on kubernetes cluster as they are planning to migrate some of their existing applications there. Recently one of the team members has been assigned a task to write a template as per details mentioned below:



Create a ReplicaSet using httpd image with latest tag only and remember to mention tag i.e httpd:latest and name it as httpd-replicaset.

Labels app should be httpd_app, labels type should be front-end. The container should be named as httpd-container; also make sure replicas counts are 4.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution
=======
- Create a manifest file
```
vi replicaset.yml # insert the code 
```
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
  labels:
    app: httpd_app
    tier: front-end
spec:
  # modify replicas according to your case
  replicas: 4
  selector:
    matchLabels:
      tier: front-end
  template:
    metadata:
      labels:
        tier: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
```

- Create the Replicaeset
```
kubectl create -f replicaset.yml
```

- Verify
```
kubectl get replicaset
```
