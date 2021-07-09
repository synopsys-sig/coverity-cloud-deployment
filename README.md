# Coverity platform deployment

This document describes a deployment process of coverity platform on docker swarm.

Deployment will use standard postgresq container as the database provider.

## Requirements

* Linux system with sufficient amount of resources.
* Docker installed (see https://docs.docker.com/engine/install/)
* Docker swarm initalized

## Deploy containers

Examine docker-comp[os.yml file and adjust resource allocation as necessary.

Deploy coverity platform with the following command:


```

docker stack deploy -c docker-compose.yml coverity

```

This will result in two running containers:

* postgres
* coverity


# System initalization

After deployment the database would have to be initalized. This is a one time action necessary for clean deployment only.

##  Database initalization


Connect to the coverity container:

```

docker exec -it $(docker ps | grep cov-connect | awk '{print $1}') bash

```

this assumes that coverity container is named cov-connect.
At the command prompt execute the following commands:

```
psql -h $DB_HOST -U postgres

```
At the prompt execute following ststements:

```

CREATE USER coverity LOGIN PASSWORD 'coverity';
CREATE DATABASE coverity;
ALTER DATABASE coverity OWNER TO coverity;

\q
```

Run database initalization script:

```
PGPASSWORD=$DB_PASSWORD psql -h $DB_HOST -U $DB_USER -f cov-platform/bin/schema.sql

```

Once complete start coverity with the following command:

```

cov-im-ctl start

```

Check status with 

```

cov-im-ctl status

```

Properly running system will show status as following:

```
$ cov-im-ctl status
         Coverity Connect
==================================
Database access status:     UP
Application process status: UP
Web interface status:       UP
[coverity@0b19a76071fe ~]$ 

```

# Accessing coverity platform


Coverity will respond at http://docker-host:8080 and https://docker-host:8443




