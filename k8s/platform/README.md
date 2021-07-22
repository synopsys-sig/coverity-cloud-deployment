# Coverity Platform


## Prerequisites

** Postgres 10.x deployed and coverity database is created owned by coverty user.

## Deployment process

deploy with the following Helm comand

```
cd platform/chart
helm install coverity/coverity
```

Once coverity connect is in running state

```
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
coverity-platform-796f5b9658-9sjbx   1/1     Running   0          18h
postgres-645fd94bbb-sh4zk            1/1     Running   0          18h
$ 
```

Set up ingress means appropriate for your cluster, connect to the UI and apply the license.

