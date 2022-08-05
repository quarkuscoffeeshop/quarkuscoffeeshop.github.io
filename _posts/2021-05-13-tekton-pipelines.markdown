---
layout: post
title:  "quarkuscoffeeshop Tekton pipelines Guide"
date:   2021-05-08 12:00:00 -0500
categories: management
---

# quarkuscoffeeshop Tekton pipelines Guide

### Requirements 
**Run the command below**
```
$ cat >source.env<<EOF
CLUSERTER_DOMAIN_NAME=clustername.example.com
TOKEN=sha256~XXXXXXXXXXXX
ACM_WORKLOADS=y
AMQ_STREAMS=y
CONFIGURE_POSTGRES=n
MONGODB_OPERATOR=n
MONGODB=n
HELM_DEPLOYMENT=n
DELETE_DEPLOYMENT=false
EOF
$ podman run  -it --env-file=./source.env  quay.io/quarkuscoffeeshop/quarkuscoffeeshop-ansible:v4.10.24
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
**quarkuscoffeeshop-homeoffice-ui tekton pipline**  
[quarkuscoffeeshop-homeoffice-ui](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/quarkuscoffeeshop-homeoffice-ui/README.md)

**homeoffice-backend tekton pipline**  
[homeoffice-backend](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/homeoffice-backend/README.md)

**homeoffice-ingress tekton pipline**  
[homeoffice-backend](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/homeoffice-ingress/README.md)


## Store front microservices  

**quarkuscoffeeshop-barista tekton pipline**  
[quarkuscoffeeshop-barista](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/quarkuscoffeeshop-barista/README.md)

**quarkuscoffeeshop-counter tekton pipline**  
[quarkuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/quarkuscoffeeshop-counter/README.md)

**quarkuscoffeeshop-kitchen tekton pipline**  
[quarkuscoffeeshop-kitchen](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/quarkuscoffeeshop-kitchen/README.md)

**quarkuscoffeeshop-homeoffice-ui tekton pipline**   
[quarkuscoffeeshop-web](https://github.com/quarkuscoffeeshop/tekton-pipelines/blob/master/quarkuscoffeeshop-web/README.md)

