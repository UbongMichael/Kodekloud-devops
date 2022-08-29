 new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:

 Create a namespace named tomcat-namespace-devops.

 Create a deployment for tomcat app which should be named as tomcat-deployment-devops under the same namespace you created. Replica count should be 1, the container should be named as tomcat-container-devops, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.

 Create a service for tomcat app which should be named as tomcat-service-devops under the same namespace you created. Service type should be NodePort and nodePort should be 32227.

 Before clicking on Check button please make sure the application is up and running.

 You can use any labels as per your choice.

 Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.

 Solution

 1. Create a namespace

 `kubectl create ns tomcat-namespace-nautilus`

 2. Create a deployment yaml file called deploy.yaml

`apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-devops~
  namespace: tomcat-namespace-devops~
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
        - image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
          name: tomcat-container-devops~
          ports:
            - containerPort: 80`
---

`apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-devops
  namespace: tomcat-namespace-devops
spec:
  type: NodePort
  selector:
    app: tomcat
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 32227`
   
3. Run
`kubectl apply -f deploy.yaml`
