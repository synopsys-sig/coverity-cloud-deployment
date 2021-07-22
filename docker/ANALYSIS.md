# Deploying and using Coverity Analysis on Docker platform

## Prerequisites

* Access to Coverity Platform (Connect)
* Coverity License 
* Docker machine

## Deploy Coverity Analysis container on Docker machine

Deploy Analysis container using the following command:

```
docker run -d -v /home/ubuntu/temp:/home/coverity/workspace gsasig/cov-analysis:2021.06T sleep 1000000
```

Use appropriate location on the host machine instead of '/home/ubuntu/temp'.
This would be the location of all project files and data

## Apply coverity licence to the runnig container.

```
docker cp license.dat $(docker ps | grep gsasig/cov-analysis:2021.06T | awk '{print $1}'):/opt/sw/synopsys/coverity/cov-analysis-linux64-2021.06-SP1/bin/
```

## Analyze and submit a test project

