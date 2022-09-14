The Nautilus DevOps team is planning to set up a Nagios monitoring tool to monitor some applications, services etc. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1) Create a deployment nagios-deployment for Nagios core. The container name must be nagios-container and it must use jasonrivers/nagios image.

2) Create a user and password for the Nagios core web interface, user must be xFusionCorp and password must be LQfKeWWxWD. (you can manually perform this step after deployment)

3) Create a service nagios-service for Nagios, which must be of targetPort type. nodePort must be 30008.

You can use any labels as per your choice.

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.


Solution: 
=========
```
kubectl get all
```
Create deployment yaml file

```
vi nagios.yaml  # paste the yaml file below into it
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nagios-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nagios-core
  template:
    metadata:
      labels:
        app: nagios-core
    spec:
      containers:
        - name: nagios-container
          image: jasonrivers/nagios
---
apiVersion: v1
kind: Service
metadata:
  name: nagios-service
spec:
  type: NodePort
  selector:
    app: nagios-core
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

 Create the deployment by the command below

 ```
 kubectl create -f nagios.yaml
 ```
```
 kubectl exec -it nagios-deployment-6674945696 -- /bin/bash
```
```
 hostname
```

 Create the username and password
```
htpasswd /opt/nagios/etc/htpasswd.users xFusionCorp
```

Verification
```
curl -u xFusionCorp http://localhost/
```

