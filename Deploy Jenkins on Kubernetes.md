
The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:



1) Create a namespace jenkins

2) Create a Service for jenkins deployment. Service name should be jenkins-service under jenkins namespace, type should be NodePort, nodePort should be 30008

3) Create a Jenkins Deployment under jenkins namespace, It should be name as jenkins-deployment , labels app should be jenkins , container name should be jenkins-container , use jenkins/jenkins image , containerPort should be 8080 and replicas count should be 1.

Make sure to wait for the pods to be in running state and make sure you are able to access the Jenkins login screen in the browser before hitting the Check button.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution
==========
Create a  namespace
```
kubectl create ns jenkins
```
Create a Yaml file with the specifications 

```
vi jenkins.yaml # Input the code below
```
```
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
  namespace: jenkins
spec:
  type: NodePort
  selector:
    app: jenkins
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30008
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
  namespace: jenkins
  labels:
    app: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
        - name: jenkins-container
          image: jenkins/jenkins
          ports:
            - containerPort: 8080
```
Create the delpoyment 
```
kubectl create -f jenk.yaml 
```
Check the running pods,deployment
```
kubectl get all -n jenkins
```
Login to Jenkins portal
``` 
kubectl exec jenkins-deployment-6b6c78f968-nxlr4 -n jenkins -- curl http://localhost:8080
```
Retrieve the password for Login
```    
kubectl exec jenkins-deployment-6b6c78f968-nxlr4 -n jenkins -- cat /var/jenkins_home/secrets/initialAdminPassword
```
Access the Jenkins login with the password 
```
kubectl exec jenkins-deployment-6b6c78f968-nxlr4 -n jenkins -- curl --user admin:e55026ed78874cadb9cc13495f48xxxx http://localhost:8080
```
