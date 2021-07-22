# Using Coverity Analysis in kubernetes environment

## Deploy Analysis container

Prepare storage volume to be used by Analysis container

```
kubectl create -f analysis-storage.yml
```

Deploy Amalysis container

```
kubectl cretae -f analysis-deployment.yml
```


## Perform test analysis and submit data 

Below is an example of analysis process:

Connect to analysis container and execute the following commands

```
# use data volume
cd /home/coverity/workspace/
# Create a project
cov-manage-im --url https://your-coverity-hostname:8443 --user user --password password3000 --mode projects --add --set name:RailsGoat --set description:"Current RailsGoat release" --on-new-cert trust
# Create Stream
cov-manage-im --url https://your-coverity-hostname:8443 --user user --password password3000 --mode streams --add --set name:RailsGoat --set language:other --set description:"Current RailsGoat release"
# Associate stream with the project
cov-manage-im --url https://your-coverity-hostname:8443 --user user --password password3000 --update --name OpenSSL --insert stream:RailsGoat"
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
cov-commit-defects --dir iDir/ --url https://your-coverity-hostname:8443 --user user --password password3000 --stream RailsGoat
```
