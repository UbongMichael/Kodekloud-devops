We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image nginx:1.18 with the latest changes.

Perform a rolling update for this application and incorporate nginx:1.18 image. The deployment name is nginx-deployment

Make sure all pods are up and running after the update.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution
========
Verify the pods and deployment running
```
kubectl get all
```
Check the description of the deployment
```
kubectl describe deployment nginx-deployment-74fb588559-86mpg
```
Read the manifest of the deployment
```
kubectl get deployment nginx-deployment -oyaml
```
Output the deployment manifest to nginx.yaml
```
kubectl get deployment nginx-deployment -oyaml > nginx.yaml
```
Edit the manifest of the deployment
```
vi nginx.yaml # change to the container image 1:18
```
Delete the deployment nginx-deployment
```
kubectl delete deployment nginx-deployment
```
Create a deployment by applying the changes
```
kubectl apply -f nginx.yaml
```

Verify the deployment and  rollout status
```
 kubectl describe deployment nginx-deployment
 ```
```
kubectl rollout status deployment nginx-deployment
```

Alternatively if you want to modify container image without going through deleting, modifying and recreating the deployment steps.
```
kubectl set image deployment nginx-deployment nginx-container=nginx:1.8
```
Verify the deployment and  rollout status
```
 kubectl describe deployment nginx-deployment
 ```
```
kubectl rollout status deployment nginx-deployment
```
