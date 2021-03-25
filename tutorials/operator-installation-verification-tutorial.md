---
title: Jenkins Operator Installation Verification Tutorial
description: This tutorial explains how to verify that the Jenkins Operator is properly installed in the namespace.
---

### Check the Jenkins Operator

- When the installation is complete, verify that the operator has been properly installed by running the following command.

```execute
kubectl get csv -n my-jenkins-operator
```

You should see a similar output as below.

```output
NAME                      DISPLAY            VERSION   REPLACES                  PHASE
jenkins-operator.v0.3.0   Jenkins Operator   0.3.0     jenkins-operator.v0.2.2   Succeeded
```

**Please wait until the `PHASE` status is `Succeeded`, then continue.**

- After the installation is complete, you can check your operator's Pod by executing the below command.

```execute
kubectl get pods -n my-jenkins-operator
```

You should see a Pod beginning with 'Jenkins-operator' with Ready value '1/1' and Status 'Running' like the output as below.

```output
NAME                             READY   STATUS    RESTARTS   AGE
jenkins-operator-8876496-m2w8s   1/1     Running   0          60s
```

