# Postgres deployment in Kubernetes environment

This deployment can be used as an example and will produce useable deployment of postgres database for Coverity Platform.
It provides minimal effort system and is not production scale.

## Deploy Postgres 

Use the postgress-configmap.yml and deploy into your cluster

```
kubectl create -f postgres-configmap.yml
```

Create a storage volume to be used by postgres. This example uses "local" storageclass. 
modify to confom to you cluster specifics.

```
kubectl create -f postgres-storge.yml
```

Deploy posgres container

```
kubectl create f postgres-deployment.yml
```

Create postgres service backed by postgres deployment

```
kubectl create -f postgres-service.yml
```

Validate that postgres is running

```
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
postgres-645fd94bbb-sh4zk            1/1     Running   0          18h
```

## Configure Database

Create 'coverity' database owned by a 'coverity' user.
Connect to the database with the following command:

```
$ kubectl exec -it postgres-645fd94bbb-sh4zk  -- psql -U postgres
```

once connected execute the following SQL statements:

```
CREATE USER coverity LOGIN PASSWORD 'coverity';
CREATE DATABASE coverity;
ALTER DATABASE coverity OWNER TO coverity;

\q
```

Database should be ready for coverity deployment


