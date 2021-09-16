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

![20210916160010](https://i.imgur.com/pifxmOG.png)


![20210916160053](https://i.imgur.com/kLFLkP8.png)


![20210916160128](https://i.imgur.com/Z4MgSjG.png)
![20210916160151](https://i.imgur.com/Ppv0s8d.png)
![20210916160211](https://i.imgur.com/AfvtjCo.png)