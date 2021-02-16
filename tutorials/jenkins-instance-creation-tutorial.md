---
title: Jenkins Instance Creation tutorial
description: This tutorial explains how to deploy Jenkins instance
---

### Deploy Jenkins

After the Jenkins operator is up and running, consider the following example :

**Step 1:** Create `jenkins-instance` file:

```execute
cat <<'EOF' > jenkins-instance.yaml
apiVersion: jenkins.io/v1alpha2
kind: Jenkins
metadata:
  name: example
spec:
  master:
    containers:
    - name: jenkins-master
      image: jenkins/jenkins:lts
      imagePullPolicy: Always
      livenessProbe:
        failureThreshold: 12
        httpGet:
          path: /login
          port: http
          scheme: HTTP
        initialDelaySeconds: 80
        periodSeconds: 10
        successThreshold: 1
        timeoutSeconds: 5
      readinessProbe:
        failureThreshold: 3
        httpGet:
          path: /login
          port: http
          scheme: HTTP
        initialDelaySeconds: 30
        periodSeconds: 10
        successThreshold: 1
        timeoutSeconds: 1
      resources:
        limits:
          cpu: 1500m
          memory: 3Gi
        requests:
          cpu: "1"
          memory: 500Mi
EOF
```

**Step 2:** Apply Jenkins Custom Resource:

```execute
kubectl create -f jenkins-instance.yaml -n my-jenkins-operator
```

Sample output:

```
jenkins.jenkins.io/example created
```

**Step 3:** Watch the Jenkins instance being created:

```execute
kubectl get pods -n my-jenkins-operator
```

You will see output similar like below:

```
NAME                             READY   STATUS              RESTARTS   AGE
jenkins-example                  0/1     ContainerCreating   0          17s
jenkins-operator-8876496-xn4jv   1/1     Running             0          109s
```

It may take up to a few minutes until all the resources are created and the cluster is ready for use.

```
NAME                             READY   STATUS    RESTARTS   AGE
jenkins-example                  1/1     Running   0          82s
jenkins-operator-8876496-xn4jv   1/1     Running   0          2m54s
```

**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 , and then proceed further.**

### Connect to Jenkins

To access Jenkins externally, lets first update service to use NodePort:

**Step 1:** Execute below command to use NodePort:

```execute
kubectl get service jenkins-operator-http-example --output yaml -n my-jenkins-operator > /tmp/my-jenkins.yaml
sed -i "s/type: .*/type: NodePort/g" /tmp/my-jenkins.yaml
kubectl patch svc jenkins-operator-http-example -p "$(cat /tmp/my-jenkins.yaml)" --namespace my-jenkins-operator
```

Output:

```output
service/jenkins-operator-http-example patched
```

**Step 2:** Execute below command to update NodePort to 32379:

```execute
kubectl get service jenkins-operator-http-example --output yaml -n my-jenkins-operator > /tmp/my-jenkins.yaml
sed -i "s/nodePort: .*/nodePort: 32379/g" /tmp/my-jenkins.yaml
kubectl patch svc jenkins-operator-http-example -p "$(cat /tmp/my-jenkins.yaml)" --namespace my-jenkins-operator
```

Output:

```output
service/jenkins-operator-http-example patched
```
**Step 3:** Access Jenkins Dashboard:

Click on the <a href="http://##DNS.ip##:32379" target="_blank">http://##DNS.ip##:32379</a> to access Jenkins Dashboard.

### Get Jenkins credentials

Execute the below commands to store the `user` and `password`:

```execute
SECRET_NAME=jenkins-operator-credentials-example
```

```execute
Username=$(kubectl -n my-jenkins-operator get secret $SECRET_NAME -o 'jsonpath={.data.user}' | base64 -d)
```

```execute
Password=$(kubectl -n my-jenkins-operator get secret $SECRET_NAME -o 'jsonpath={.data.password}' | base64 -d)
```

Get the credentials:

```execute
echo $Username
```

```execute
echo $Password
```

Now use these to login to your Jenkins Dashboard.

### Login with credentials

![](_images/jenkins-login.png)

Now you will be able to create your own pipelines in Jenkins Dashboard.

![](_images/dashboard.png)

