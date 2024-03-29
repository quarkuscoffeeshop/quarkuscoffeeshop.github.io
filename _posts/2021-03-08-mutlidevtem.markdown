---
layout: post
title:  "Quarkuscoffeeshop ACM deployment for multiple dev teams"
date:   2021-03-08 09:50:00 -0500
categories: management
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


### Install Postgres Operator
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


**Update project for Dev Teams**  
`For devteam1`
```
$ sed -i 's/quarkuscoffeeshop-demo/quarkuscoffeeshop-devteam1-gitops/g' values.yaml
$ cat values.yaml | grep quarkuscoffeeshop-devteam1-gitops
$ ansible-playbook -i inventory.yaml --tags=install  main.yml
$ ansible-playbook -i inventory.yaml --tags=createdb main.yml -vv
```

`For devteam2`  
```
$ sed -i 's/quarkuscoffeeshop-devteam1-gitops/quarkuscoffeeshop-devteam2-gitops/g' values.yaml
$ cat values.yaml | grep quarkuscoffeeshop-devteam2-gitops
$ ansible-playbook -i inventory.yaml --tags=install  main.yml
$ ansible-playbook -i inventory.yaml --tags=createdb main.yml -vv
```

`For devteam3`
```
$ sed -i 's/quarkuscoffeeshop-devteam2-gitops/quarkuscoffeeshop-devteam3-gitops/g' values.yaml
$ cat values.yaml | grep quarkuscoffeeshop-devteam3-gitops
$ ansible-playbook -i inventory.yaml --tags=install  main.yml
$ ansible-playbook -i inventory.yaml --tags=createdb main.yml -vv
```

*Note: if the database deployment fails for devteam2 and devteam3 run the following below*  
*TO-DO: fix in script*  
```
$ ansible-playbook -i inventory.yaml --tags=uninstall  main.yml
$ rm -rf ~/.pgo/quarkuscoffeeshop-devteam#-gitops/
$ ansible-playbook -i inventory.yaml --tags=install  main.yml
$ ansible-playbook -i inventory.yaml --tags=createdb main.yml -vv
```

*The password is saved under `cat /tmp/postgres-info.txt`*. 
*The password will also be found in the `TASK [pgo-operator : Configure Database cluster]` output*. 

For more Postgress deployment Options see [postgres-operator](https://github.com/tosin2013/postgres-operator)


### Run ansible playbook to install Red Hat AMQ and Configure Postgres on target cluster
```
$ curl -OL https://raw.githubusercontent.com/quarkuscoffeeshop/quarkuscoffeeshop-ansible/master/files/deploy-quarkuscoffeeshop-ansible.sh
$ chmod +x deploy-quarkuscoffeeshop-ansible.sh
$  sed -i 's/quarkuscoffeeshop-demo/quarkuscoffeeshop-devteam1-gitops/g' deploy-quarkuscoffeeshop-ansible.sh
$ ./deploy-quarkuscoffeeshop-ansible.sh  -d ocp4.example.com -o sha-123456789 -p '123456789' -s DEVTEAM1
$  sed -i  's/quarkuscoffeeshop-devteam1-gitops/quarkuscoffeeshop-devteam2-gitops/g' deploy-quarkuscoffeeshop-ansible.sh
$ ./deploy-quarkuscoffeeshop-ansible.sh  -d ocp4.example.com -o sha-123456789 -p '123456789' -s DEVTEAM2
$  sed -i  's/quarkuscoffeeshop-devteam2-gitops/quarkuscoffeeshop-devteam3-gitops/g' deploy-quarkuscoffeeshop-ansible.sh
$ ./deploy-quarkuscoffeeshop-ansible.sh  -d ocp4.example.com -o sha-123456789 -p '123456789' -s DEVTEAM3
```

### Follow the Instructions configure the deploymnents for teams
* [https://github.com/quarkuscoffeeshop/acm-multi-tenancy](https://github.com/quarkuscoffeeshop/acm-multi-tenancy)

### Follow the Instructions for each dev team
* [https://github.com/quarkuscoffeeshop/acm-multi-tenancy-dev-team](https://github.com/quarkuscoffeeshop/acm-multi-tenancy-dev-team)
