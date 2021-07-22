# Using Coverity deployment on Kubernetes

## Requirements

* Kubernetes cluster
* Horking kubectl configured to work with the cluster
* Helm
* Coverity License

## Deployment process

### Deploy and configure the database

Coverity platform requires Postgres database to be deployed and configured with database and a database user to be used by Coverity.

For an example on how to deploy standard postgres containe see [this README file](postgres/README.md)

## Deploy Coverity Platform

Coverity Platform is deployed using provided helm chart. Chart located in [platform/chart](platform/chart) folder
see [README](platform/README.md)  file for details

## Deploy Analysis 

For an example on how to deploy and use Coverity Analysis see [this README file](analysis/README.md)
