---
layout: post
title:  "quarkuscoffeeshop Tekton pipelines Guide"
date:   2021-09-15 12:00:00 -0500
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
CONFIGURE_POSTGRES=y
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

## Store front microservices
> This will build the development environment for the quarkuscoffeeshop store.

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
* Organization Name: `quarkuscoffeeshop`
![20210916161629](https://i.imgur.com/vspBsUU.png)
![20210916161651](https://i.imgur.com/3MyCD3B.png)


### update the deploy-pipeline.yaml for the following services  in gogs
**quarkuscoffeeshop-barista tekton pipeline**  

```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/quarkuscoffeeshop-barista
```

**quarkuscoffeeshop-counter tekton pipeline**  

```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/quarkuscoffeeshop-counter
```

**quarkuscoffeeshop-kitchen tekton pipeline**  

```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/quarkuscoffeeshop-kitchen
```

**quarkuscoffeeshop-web tekton pipeline**   

```
    # Change to internal quay repo
    - default: quay.io/quarkuscoffeeshop/quarkuscoffeeshop-web
```


### update the transformer-patches.yaml for each microservice
* quarkuscoffeeshop-counter
![20210918121934](https://i.imgur.com/9OHN25l.png)
* quarkuscoffeeshop-web
> CHANGEME=quarkuscoffeeshop-web-quarkuscoffeeshop-demo.apps.ocp4.example.com
![20210918122147](https://i.imgur.com/5MCsWQE.png)


### To access Argocd
![20210916163958](https://i.imgur.com/9jORJOA.png)

![20210916164028](https://i.imgur.com/qlrPOuJ.jpg)

The password for argocd is found under the following secret
![20210916164140](https://i.imgur.com/iiuHTxl.png)


### Login to argocd 
![20210916164230](https://i.imgur.com/7FhIxh5.png)
![20210916164312](https://i.imgur.com/aKONE3S.jpg)

### Terminal clone your git repo
```
git clone http://gogs-quarkuscoffeeshop-cicd.apps.ocp4.example.com/user1/tekton-pipelines.git
```
### login to OpenShift cluster 
```
oc login --token=sha256~yoursha --server=https://api.ocp4.example.com:6443
```

### cd into tekton pipelines folder 
```
cd tekton-pipelines
```

## update repo url 
```
REPO_URL='http://gogs-quarkuscoffeeshop-cicd.apps.ocp4.example.com/user1/tekton-pipelines.git'
```
### load the argocd application templates for each microservice
**quarkuscoffeeshop-barista argo application**  
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/quarkuscoffeeshop-barista/quarkuscoffeeshop-barista-template.yaml  > argocd/quarkuscoffeeshop-barista/quarkuscoffeeshop-barista.yaml
oc create -f argocd/quarkuscoffeeshop-barista/quarkuscoffeeshop-barista.yaml  -n openshift-gitops
```

**quarkuscoffeeshop-counter argo application**  
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/quarkuscoffeeshop-counter/quarkuscoffeeshop-counter-template.yaml  > argocd/quarkuscoffeeshop-counter/quarkuscoffeeshop-counter.yaml
oc create -f argocd/quarkuscoffeeshop-counter/quarkuscoffeeshop-counter.yaml  -n openshift-gitops
```

**quarkuscoffeeshop-kitchen argo application**  
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/quarkuscoffeeshop-kitchen/quarkuscoffeeshop-kitchen-template.yaml  > argocd/quarkuscoffeeshop-kitchen/quarkuscoffeeshop-kitchen.yaml
oc create -f argocd/quarkuscoffeeshop-kitchen/quarkuscoffeeshop-kitchen.yaml  -n openshift-gitops
```

**quarkuscoffeeshop-web argo application**   
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/quarkuscoffeeshop-web/quarkuscoffeeshop-web-template.yaml  > argocd/quarkuscoffeeshop-web/quarkuscoffeeshop-web.yaml
oc create -f argocd/quarkuscoffeeshop-web/quarkuscoffeeshop-web.yaml  -n openshift-gitops
```

![20210918122954](https://i.imgur.com/RDN8MuA.png)


### To run pipelines go to the quarkuscoffeeshop-cicd project 
![20210918123135](https://i.imgur.com/Xi5gFxx.png)

**To start the build-and-push-quarkuscoffeeshop-barista pipeline**
![20210918123445](https://i.imgur.com/4nrBVKx.png)
![20210918123532](https://i.imgur.com/hntPWfb.png)

**To start the build-and-push-quarkuscoffeeshop-counter pipeline**
![20210918123622](https://i.imgur.com/31Bg10i.png)
![20210918123709](https://i.imgur.com/qfn5puz.png)

**To start the build-and-push-quarkuscoffeeshop-kitchen pipeline**
![20210918131559](https://i.imgur.com/wxM1RMY.png)
![20210918131650](https://i.imgur.com/AOrOIrQ.png)

**To start the build-and-push-quarkuscoffeeshop-web pipeline**
![20210918131734](https://i.imgur.com/CixJOul.png)
![20210918131809](https://i.imgur.com/XglAdc5.png)

### Deployment Validation 
WIP
