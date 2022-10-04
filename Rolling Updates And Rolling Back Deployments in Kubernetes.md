There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.



Create a namespace nautilus. Create a deployment called httpd-deploy under this new namespace, It should have one container called httpd, use httpd:2.4.25 image and 3 replicas. The deployment should use RollingUpdate strategy with maxSurge=1, and maxUnavailable=2. Also create a NodePort type service named httpd-service and expose the deployment on nodePort: 30008.

Now upgrade the deployment to version httpd:2.4.43 using a rolling update.

Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution
========
alias k=kubectl
- Create namespace
k create ns nautilus
- Create a deployment file using imperative commands
```
kubectl create deployment httpd-deploy -n nautilus --image=httpd:2.4.25 --replicas=3 --dry-run=client -oyaml > deploy.yml
```
- Edit the deploy.yml # Add RollingUpdate strategy with maxSurge and maxUnavailable. Ensure the app name is consistent
```
vi deploy.yml 
```
- Add this code to the deploy.yml
```
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
```
- Create a service file and expose the nodeport using imperative commands.
```
kubectl create service nodeport httpd --node-port=30008 --tcp=80:80 -n nautilus --dry-run=client -oyaml > svc.yml
```
- Edit the svc.yml file
```
vi svc.yml # change the service name
```
- Create deployment
```
kubectl create -f deploy.yml 
```
- Create service
```
kubectl create -f svc.yml 
```
- Verify that deployment, service and pod are up and running
```
kubectl get svc -n nautilus
kubectl get deploy -n nautilus
kubectl get po -n nautilus
```
- Upgrade the deployment to version httpd:2.4.43 using a rolling update
```
kubectl set image deployment httpd-deploy -n nautilus httpd=httpd:2.4.43 --record
```
- Check the status of the deployment
```
kubectl rollout status deployment httpd-deploy -n nautilus
```
- Check the history of the deployment
```
kubectl rollout history deployment httpd-deploy -n nautilus
```
- Roll back to the previous/original version
```
kubectl rollout undo deployment httpd-deploy -n nautilus --to-revision=1
```
- Check the status of the rollback deployment
```
kubectl rollout status deployment httpd-deploy -n nautilus
```
