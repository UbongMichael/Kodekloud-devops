We deployed a Nginx and PHPFPM based application on Kubernetes cluster last week and it had been working fine. This morning one of the team members was troubleshooting an issue with this stack and he was supposed to run Nginx welcome page for now on this stack till issue with phpfpm is fixed but he made a change somewhere which caused some issue and the application stopped working. Please look into the issue and fix the same:

The deployment name is nginx-phpfpm-dp and service name is nginx-service. Figure out the issues and fix them. FYI Nginx is configured to use default http port, node port is 30008 and copy index.php under /tmp/index.php to deployment under /var/www/html. Please do not try to delete/modify any other existing components like deployment name, service name etc.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution
=======

```
kubectl get deployment
```
```
kubectl get po
```
```
kubectl get svc
```
```
kubectl get cm
```
```
kubectl describe cm nginx-config
```
- Add index.php under (Set nginx to serve files from the shared volume!)

```
kubectl edit cm nginx-config
```
```
kubectl get svc
```
- Need to change the port number in svc by using find command (/8099)

```
kubectl edit svc nginx-service
```
```
kubectl get svc
```
```
kubectl get po
```
-  Acess the app on the web. Still not working

```
kubectl cp /tmp/index.php nginx-phpfpm-dp-5cccd45499-5r2pr:/var/www/html
```
```
kubectl get po
```
```
kubectl logs nginx-phpfpm-dp-5cccd45499-5r2pr -c nginx-container
```
```
kubectl get cm
```
```kubectl describe cm
```
-  Check before restart and still not working 

```
kubectl get po
```
```
kubectl get deploy
```
```
kubectl rollout restart deployment nginx-phpfpm-dp
```
-  Now we need to copy that index.php file again
```
kubectl get po
```
```kubectl cp /tmp/index.php pod-name:.var/www/html -c nginx-container
```
```
kubectl cp /tmp/index.php nginx-phpfpm-dp-6c6d8b4df4-mz5gw:/var/www/html -c nginx-container
```
- All good now, access the app
