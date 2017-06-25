# Openshift Setup Commands in local

- Install "minishift" if not exist
```
brew cask install minishift
```

- Predefine Config settings for Openshift and verfiy
```
openshift_version=v1.5.0-alpha.3
minishift config set memory 7168
minishift config set cpus 4
minishift config set v 999
minishift config set openshift-version $openshift_version
minishift config view
```

- Start minishift with predefined version
```
minishift start --openshift-version=$openshift_version --logtostderr --show-libmachine-logs
minishift status
```

- Include "oc" in path
```
f=~/.bash_profile
echo "export PATH=~/.minishift/cache/oc/$openshift_version:\$PATH" >> $f
source $f
echo $PATH
```

- Determine environment and IP address of cluster
```
eval $(minishift docker-env)
```

- Login into cluster
```
oc login https://$(minishift ip):8443 -u developer -p developer
oc new-project <<New Project>>
oc policy add-role-to-user admin developer -n <<New Project>>
oc policy add-role-to-user view -n <<New Project>> -z default
```
- Issue with Vert.x conainer, related to cache dir is created
```
oc adm policy add-scc-to-user anyuid -z default
```
