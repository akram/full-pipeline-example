# full-pipeline-example

## Create custom slave images

In a specific project
```
oc new-project jenkins-custom-slaves
```

### NodeJS 6
```
oc new-build  --strategy=docker --name=jenkins-slave-nodejs6 --code=https://github.com/akram/jenkins-slave-nodejs6.git
```

### Maven 3.5
```
oc new-build  --strategy=docker --name=jenkins-slave-maven35 --code=https://github.com/akram/jenkins-slave-maven35.git
```

## Use Jenkins Blue Ocean
```
oc new-project jenkins-blue-ocean
oc new-build jenkins:2~https://github.com/siamaksade/jenkins-blueocean.git --name=jenkins-blueocean
```

## Create pipeline project
Create another project
```
oc new-project full-pipeline
```

Give permission to users from full-pipeline to pull images from jenkins-custom-slaves and jenkins-blue-ocean
```
oc policy add-role-to-user system:image-puller system:serviceaccount:full-pipeline:default -n jenkins-custom-slaves
oc policy add-role-to-user system:image-puller system:serviceaccount:full-pipeline:default -n jenkins-blue-ocean
```

Tag images to use as custom-slave in Jenkins
```
oc tag jenkins-custom-slaves/jenkins-slave-nodejs6:latest nodejs6:latest
oc tag jenkins-custom-slaves/jenkins-slave-maven35:latest maven35:latest
oc label is nodejs6 role=jenkins-slave
oc label is maven35 role=jenkins-slave
```

Instanciate jenkins-blue-ocean
```
oc new-app jenkins-ephemeral -p NAMESPACE=jenkins-blue-ocean -p JENKINS_IMAGE_STREAM_TAG=jenkins-blueocean:latest 
```







