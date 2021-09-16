---
layout: post
title:  "quarkuscoffeeshop Tekton pipelines Guide"
date:   2021-05-08 12:00:00 -0500
categories: devops
---

# quarkuscoffeeshop Tekton pipelines Guide

### Requirements 
* [Postgres Operator](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/wiki#install-postgres-operator)
```
$ curl -OL https://raw.githubusercontent.com/tosin2013/postgres-operator/main/scripts/deploy-postgres-operator.sh
$ chmod +x deploy-postgres-operator.sh
$ ./deploy-postgres-operator.sh 
./deploy-postgres-operator.sh [OPTION]
 Options:
  -d      Add domain 
  -t      OpenShift Token
  -u      Uninstall deployment
  To deploy postgres-operator playbooks
  ./deploy-postgres-operator.sh  -d ocp4.example.com -o sha-123456789 
  To Delete postgres-operator playbooks from OpenShift
  ./deploy-postgres-operator.sh  -d ocp4.example.com -o sha-123456789 -u true
```

**Once Postgres Operator Database is installed run the following below**
```
$ cat >env.variables<<EOF
ACM_WORKLOADS=y
AMQ_STREAMS=y
CONFIGURE_POSTGRES=n
MONGODB_OPERATOR=n
MONGODB=n
HELM_DEPLOYMENT=n
EOF
$ ./deploy-quarkuscoffeeshop-ansible.sh -d ocp4.example.com -t sha-123456789 -p 123456789 -s ATLANTA
```

**Install tkn cli**  
`on linux`
```
# Get the tar.xz
curl -LO https://github.com/tektoncd/cli/releases/download/v0.20.0/tkn_0.20.0_Linux_x86_64.tar.gz
# Extract tkn to your PATH (e.g. /usr/local/bin)
sudo tar xvzf tkn_0.20.0_Linux_x86_64.tar.gz -C /usr/local/bin/ tkn
```

`on mac`
```
brew install tektoncd-cli
```


## HOME Office (Backoffice)

### The gogs route is found under the quarkuscoffeeshop-cicd project
![20210916160010](https://i.imgur.com/pifxmOG.png)
![20210916160053](https://i.imgur.com/kLFLkP8.png)

### Use the the following to login
* username: `user1`
* password: `openshift`
![20210916160128](https://i.imgur.com/Z4MgSjG.png)
![20210916160151](https://i.imgur.com/Ppv0s8d.png)

### Navigate to the `tekton-pipelines` repo
![20210916160211](https://i.imgur.com/AfvtjCo.png)

### The quay registry is found under the `quay-enterprise` project
![20210916160943](https://i.imgur.com/uJskjul.png)
![20210916161424](https://i.imgur.com/1NisymA.png)

### Create an admin user for the deployment
* username: `admin`
* email address: `admin@example.com`
* password: `admin123`
![20210916161516](https://i.imgur.com/Iaaa9eU.png)
![20210916161551](https://i.imgur.com/eZDmbvV.png)

### Create Quarkuscoffeeshop organization for images
![20210916161629](https://i.imgur.com/vspBsUU.png)
![20210916161651](https://i.imgur.com/3MyCD3B.png)


### update the deploy-pipeline.yaml for the following services  in gogs
* homeoffice-ingress
```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/homeoffice-ingress
```
* homeoffice-backend
```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/homeoffice-backend
```
* quarkuscoffeeshop-homeoffice-ui
```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/quarkuscoffeeshop-homeoffice-ui
    # change to your url 
    - default: >-
    http://quarkuscoffeeshop-homeoffice-ui-quarkuscoffeeshop-homeoffice.apps.shop.example.com/graphql
```

### update the transformer-patches.yaml for each microservice
* homeoffice-ingress
> this is for the postgres password you can find this at the bottom odd the doc
![20210916162845](https://i.imgur.com/A9OAGla.png)
* homeoffice-backend
> this is for the postgres password you can find this at the bottom odd the doc
![20210916162812](https://i.imgur.com/Ho5jqi9.png)

### update the quarkuscoffeeshop-homeoffice-ui-route.yaml for quarkuscoffeeshop-homeoffice-ui
* quarkuscoffeeshop-homeoffice-ui
![20210916163055](https://i.imgur.com/s3fXANW.png)


## Postgres password location 
![20210916163339](https://i.imgur.com/VkG7Siu.png)
