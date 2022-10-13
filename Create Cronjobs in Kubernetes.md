There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:

Create a cronjob named datacenter.

Set schedule to */9 * * * *.

Container name should be cron-datacenter.

Use nginx image with latest tag only and remember to mention the tag i.e nginx:latest.

Run a dummy command echo Welcome to xfusioncorp!.

Ensure restart policy is OnFailure.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster

Solution
========
```
kubectl get pod
```
```
kubectl get cronjob
```
```
vi /tmp/cron.yml
```

```
apiVersion: batch/v1

kind: CronJob

metadata:

  name: hello

spec:

  schedule: "* * * * *"

  jobTemplate:

    spec:

      template:

        spec:

          containers:

          - name: hello

            image: nginx:latest

            imagePullPolicy: IfNotPresent

            command:

            - /bin/sh

            - -c

            - echo Welcome to xfusioncorp!

          restartPolicy: OnFailure
```
```
kubectl create -f /tmp/cron.yml 
```
```
kubectl get po
```
```
kubectl get cj
```
```
kubectl get po --watch
```
```
kubectl logs datacenter-1665647640-rwzj
```
