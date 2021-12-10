---
layout: post
title:  "homeoffice tekton pipelines Guide"
date:   2021-09-16 12:00:00 -0500
categories: devops
---

# quarkuscoffeeshop homeoffice Tekton pipelines Guide

### Requirements 
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
* Organization Name: `quarkuscoffeeshop`
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
> this is for the postgres password you can find this at the bottom off the doc
![20210916162845](https://i.imgur.com/A9OAGla.png)
* homeoffice-backend
> this is for the postgres password you can find this at the bottom off the doc
![20210916162812](https://i.imgur.com/Ho5jqi9.png)

### update the quarkuscoffeeshop-homeoffice-ui-route.yaml for quarkuscoffeeshop-homeoffice-ui
![20210916163055](https://i.imgur.com/s3fXANW.png)

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
**quarkuscoffeeshop-homeoffice-ui argo application**  
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/quarkuscoffeeshop-homeoffice-ui/quarkuscoffeeshop-homeoffice-ui-template.yaml  > argocd/quarkuscoffeeshop-homeoffice-ui/quarkuscoffeeshop-homeoffice-ui.yaml
oc create -f argocd/quarkuscoffeeshop-homeoffice-ui/quarkuscoffeeshop-homeoffice-ui.yaml  -n openshift-gitops
```

**homeoffice-backend argo application**  
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/homeoffice-backend/homeoffice-backend-template.yaml  > argocd/homeoffice-backend/homeoffice-backend.yaml
oc create -f argocd/homeoffice-backend/homeoffice-backend.yaml  -n openshift-gitops
```

**homeoffice-ingress argo application**  
```
sed "s|%REPO_NAME%|'${REPO_URL}'|g" argocd/homeoffice-ingress/homeoffice-ingress-template.yaml  > argocd/homeoffice-ingress/homeoffice-ingress.yaml
oc create -f argocd/homeoffice-ingress/homeoffice-ingress.yaml  -n openshift-gitops
```

![20210916164929](https://i.imgur.com/GnRGx38.png)

### To run pipelines go to the quarkuscoffeeshop-cicd project 
![20210916165144](https://i.imgur.com/GUk6oEj.png)

**To start the build-and-push-homeoffice-backend pipeline** 
![20210916165411](https://i.imgur.com/V5TkeKM.png)
![20210916165442](https://i.imgur.com/9yhWTJt.png)

**To start the build-and-push-homeoffice-ingress pipeline** 
![20210916165559](https://i.imgur.com/80TKQSQ.png)
![20210916165630](https://i.imgur.com/KFYA8cF.png)

**To start the build-and-push-quarkuscoffeeshop-homeoffice-ui pipeline**
![20210916165749](https://i.imgur.com/s8AswNF.png)
![20210916165816](https://i.imgur.com/pQipUCq.png)

### Deployment Validation 
#### Validate pipeline completes
![20210917144852](https://i.imgur.com/zebme7M.png)

#### verify images have been pushed to quay
![20210917144806](https://i.imgur.com/Zf00MBm.png)

#### Verify deployment is successful 
![20210917144949](https://i.imgur.com/JVXg7Pe.png)

#### Access to homeoffice ui
![20210917145049](https://i.imgur.com/3nQNMBy.png)
![20210917145109](https://i.imgur.com/BDUxRfI.png)

## Postgres password location 
![20210916163339](https://i.imgur.com/VkG7Siu.png)
