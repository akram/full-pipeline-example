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

## Create pipeline project
In another project
```
oc new-project jenkins-custom-slaves
```







