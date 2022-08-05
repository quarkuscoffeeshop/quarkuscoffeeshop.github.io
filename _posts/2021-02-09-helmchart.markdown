---
layout: post
title:  "Quarkuscoffeeshop Helm Deployment"
date:   2021-02-09 08:59:35 -0500
categories: helm deployment
---

# Deploy QuarkusCoffeeShop v5.0 Using a helm chart
![Lint and Test Charts](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/workflows/Lint%20and%20Test%20Charts/badge.svg)
![Release Charts](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/workflows/Release%20Charts/badge.svg)


## Administrator Tasks 
    
**Install Helm**

```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

### OpenShiftt 4.x  Deployment Instructions 

### Run ansible playbook to install Red Hat AMQ and Configure Postgres on target cluster
```
$ cat >source.env<<EOF
CLUSERTER_DOMAIN_NAME=clustername.example.com
TOKEN=sha256~XXXXXXXXXXXX
ACM_WORKLOADS=n
AMQ_STREAMS=y
CONFIGURE_POSTGRES=y
MONGODB_OPERATOR=n
MONGODB=n
HELM_DEPLOYMENT=n
DELETE_DEPLOYMENT=false
EOF
$ podman run  -it --env-file=./source.env  quay.io/quarkuscoffeeshop/quarkuscoffeeshop-ansible:v4.10.24
```

### To deploy quarkuscoffeeshop using a helm chart
**Add the Quarkus Coffee Shop  Helm Chart repository**
```
helm repo add quarkuscoffeeshop https://quarkuscoffeeshop.github.io/quarkuscoffeeshop-helm/
```

**Check helm repo**
```
$ helm repo list
NAME             	URL
quarkuscoffeeshop	https://quarkuscoffeeshop.github.io/quarkuscoffeeshop-helm/
```

**Test the helm repo**
```
helm search repo quarkuscoffeeshop
```

**Configure values.yml**
```
cat >values.yml<<EOF
# Quarkus Cafe Application Variables
projectnamespace: <<NAMESPACE>>
domain: <<DOMAIN>>
kafka_cluster_name: cafe-cluster
pgsql_password: <<CHANGEPASSWORD>>
storeid: <<CHANGESTOREID>>
# Helm chart variables
Release:
  Name: quarkuscoffeeshop-deployment
  release-namespace: <<NAMESPACE>>
EOF
```

**Deploy the Cluster Operator using the Helm command line tool**
```
# Run a test of deployment
helm install quarkuscoffeeshop/quarkuscoffeeshop-charts --generate-name --values values.yml --dry-run

# Deploy quarkuscoffeeshop
helm install quarkuscoffeeshop/quarkuscoffeeshop-charts --generate-name --values values.yml
```

**Expose routes for Web and Customermocker**

The Helm chart does not currently expose the routes for our Web UI or the Customermocker service.  You will need to log in and expose them using oc:

``` 
oc expose svc/quarkuscoffeeshop-web
oc expose svc/quarkuscoffeeshop-customermocker
```

**Uninstall quarkus application**
```
helm uninstall quarkus-cafe-deployment
```

## Helm contribution and development instructions
**Git clone repo to laptop**
```
git  clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm.git
```

**cd into quarkuscoffeeshop-helm/charts**
```
cd quarkuscoffeeshop-helm/charts
```

#### Development Tips
* **Update the default values found in `quarkuscoffeeshop-helm/charts/quarkuscoffeeshop-charts/values.yaml`**. 

* **Optional: Edit files in the `quarkuscoffeeshop-helm/charts/quarkuscoffeeshop-charts/templates` folder to modify the deployment configs**. 

**Test Install quarkus application**
```
helm install  quarkuscoffeeshop-deployment  quarkuscoffeeshop-charts/ --values quarkuscoffeeshop-charts/values.yaml  --dry-run
```

**Install quarkus application**
```
helm install  quarkuscoffeeshop-deployment  quarkuscoffeeshop-charts/ --values quarkuscoffeeshop-charts/values.yaml
```

**Expose routes for Web and Customermocker**

*The Helm chart does not currently expose the routes for our Web UI or the Customermocker service.  You will need to log in and expose them using oc:*

```
oc expose svc/quarkuscoffeeshop-web
oc expose svc/quarkuscoffeeshop-customermocker
```

**Uninstall quarkus application**
```
helm uninstall  quarkuscoffeeshop-deployment
```
