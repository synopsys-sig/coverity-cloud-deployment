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

For this example it is assumed that Coverity Platform and Coverity Analysis are running on the same Docker host.

Connect to analysis container 

```
docker exec -it $(docker ps | grep gsasig/cov-analysis:2021.06T | awk '{print $1}') bash
```

Execute the following commands in the container. Use appropriate hostname, username and password instead of "docker-host", "user" and "password"

```
# use data volume
cd /home/coverity/workspace/
# Create a project
cov-manage-im --url https://docker-host:8443 --user user --password password --mode projects --add --set name:RailsGoat --set description:"Current RailsGoat release" --on-new-cert trust
# Create Stream
cov-manage-im --url https://docker-host:8443 --user user --password password --mode streams --add --set name:RailsGoat --set language:other --set description:"Current RailsGoat release" --on-new-cert trust
# Associate stream with the project
cov-manage-im --url https://docker-host:8443 --user user --password password --mode projects --update --name RailsGoat --insert stream:RailsGoat
# Get project source
git clone https://github.com/OWASP/railsgoat.git
cd railsgoat/
# Create Ruby configuration
cov-configure --ruby
# Run Coverity Analysis process
cov-build --dir iDir/ --no-command --fs-capture-search .
cov-import-scm --dir iDir/ --scm git --project-root .
cov-analyze --dir iDir/ --aggressiveness-level low --all --disable-fb --distrust-all --enable-audit-mode --strip-path `pwd` --webapp-security --webapp-security-aggressiveness-level low
# Submit results
cov-commit-defects --dir iDir/ --url https://docker-host:8443 --user user --password foobar --stream RailsGoat
```
