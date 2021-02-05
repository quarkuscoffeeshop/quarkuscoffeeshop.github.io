---
layout: page
title: DevOps
permalink: /devops/
---


# Deploy Quarkus Cafe 5.0 Using a helm chart
![Lint and Test Charts](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/workflows/Lint%20and%20Test%20Charts/badge.svg)
![Release Charts](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/workflows/Release%20Charts/badge.svg)

## Administrator Tasks 
    
**Install Helm**

```shell script
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

### OpenShiftt 4.x  Deployment Instructions 

**Login to OpenShift and create project**
```
oc new-project <<NAMESPACE>
```

### Install Postgres Operator
**switch to root user**
```
sudo su - root
```
**cd into temp directory**
```
cd /tmp
```

**git clone postgres-operator**
```
$ git clone https://github.com/tosin2013/postgres-operator.git
$ cd postgres-operator/
```

**Create inventory file**
```
$ export DOMAIN=ocp4.example.com
$ export OCP_TOKEN=123456789
$ cat >inventory.yaml<<YAML
---
  all:
    hosts:
        localhost:
    vars:
        ansible_connection: local
        config_path: "{{ playbook_dir }}/values.yaml"
        # ==================
        # Installation Methods
        # One of the following blocks must be updated:
        # - Deploy into Kubernetes
        # - Deploy into Openshift

        # Deploy into Kubernetes
        # ==================
        # Note: Context name can be found using:
        #   kubectl config current-context
        # ==================
        # kubernetes_context: ''

        # Deploy into Openshift
        # ==================
        # Note: openshift_host can use the format https://URL:PORT
        # Note: openshift_token can be used for token authentication
        # ==================
        openshift_host: 'https://api.${DOMAIN}:6443'
        openshift_skip_tls_verify: true
        # openshift_user: ''
        # openshift_password: ''
        openshift_token: '${OCP_TOKEN}'
YAML
```

**Install Postgres Database Operator**
```
ansible-playbook -i inventory.yaml --tags=install  main.yml
```

**Create Database Cluster**

```
ansible-playbook -i inventory.yaml --tags=createdb main.yml -vv
```
*The password is saved under `cat /tmp/postgres-info.txt`*. 
*The password will also be found in the `TASK [pgo-operator : Configure Database cluster]` output*. 

For more Postgress deployment Options see [postgres-operator](https://github.com/tosin2013/postgres-operator)


### Run ansible playbook to install Red Hat AMQ and Configure Postgres on target cluster
```
$ ansible-galaxy collection install community.kubernetes
$ ansible-galaxy install tosin2013.quarkus_cafe_demo_role
$ export DOMAIN=ocp4.example.com
$ export OCP_TOKEN=123456789
$ export POSTGRES_PASSWORD=123456789
$ export STORE_ID=ATLANTA
$ cat >deploy-quarkus-cafe.yml<<YAML
- hosts: localhost
  become: yes
  vars:
    openshift_token: ${OCP_TOKEN}
    openshift_url: https://api.${DOMAIN}:6443
    insecure_skip_tls_verify: true
    default_owner: ${USER}
    default_group: ${USER}
    project_namespace: quarkuscoffeeshop-demo
    delete_deployment: false
    skip_amq_install: false
    skip_configure_postgres: false
    skip_mongodb_operator_install: true
    skip_quarkuscoffeeshop_helm_install: true
    domain: ${DOMAIN}
    postgres_password: "${POSTGRES_PASSWORD}"
    storeid: ${STORE_ID}
  roles:
    - tosin2013.quarkus_cafe_demo_role
YAML
$ ansible-playbook  deploy-quarkus-cafe.yml
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

```shell 
oc expose svc/quarkuscoffeeshop-web
oc expose svc/quarkuscoffeeshop-customermocker
```

**Uninstall quarkus application**
```shell
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

```shell script
oc expose svc/quarkuscoffeeshop-web
oc expose svc/quarkuscoffeeshop-customermocker
```

**Uninstall quarkus application**
```
helm uninstall  quarkuscoffeeshop-deployment
```
