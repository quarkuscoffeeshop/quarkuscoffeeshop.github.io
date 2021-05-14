---
layout: page
title: About
permalink: /about/
---

![My helpful screenshot](_images_/webpage-example.png)

## Quarkus Coffee Shop
---
Quarkus Coffee Shop can be used to demo different Red Hat products and technology. The technologies used are below.

* **Quarkus** - [Quarkus](https://www.redhat.com/en/topics/cloud-native-apps/what-is-quarkus) is a full-stack, Kubernetes-native, Java Application Framework tailored for OpenJDK HotSpot and GraalVM.
* **Apache Kafka (AMQ Streams)** -  [Red Hat AMQ streams](https://www.redhat.com/en/resources/amq-streams-datasheet) The Red Hat® AMQ streams component is a massively scalable, distributed, and high-performance data streaming platform based on the Apache Kafka project. 
* **Debezium** - [Debezium](https://debezium.io/) Debezium is an open source distributed platform for change data capture.
* **nodejs** - [nodejs](https://developers.redhat.com/blog/2020/01/07/red-hat-support-for-node-js#red_hat_node_js_experts_at_your_fingertips) Node.js is an open-source, cross-platform, back-end JavaScript runtime environment that runs on the V8 engine and executes JavaScript code outside a web browser.
* **Graphql** - [GraphQL](https://www.redhat.com/en/topics/api/what-is-graphql) is an open-source data query and manipulation language for APIs, and a runtime for fulfilling queries with existing data.
* **PostgreSQL** - [PostgreSQL](https://www.redhat.com/sysadmin/postgresql-setup-use-cases), also known as Postgres, is a free and open-source relational database management system emphasizing extensibility and SQL compliance. 
* **OpenShift** - [OpenShift](https://www.openshift.com/) Red Hat OpenShift is an open source container application platform based on the Kubernetes container orchestrator for enterprise app development and deployment in the hybrid cloud
* **Red Hat Advanced Cluster Management (ACM)** - [Red Hat® Advanced Cluster Management for Kubernetes](https://www.redhat.com/en/technologies/management/advanced-cluster-management) controls clusters and applications from a single console, with built-in security policies. Extend the value of Red Hat OpenShift® by deploying apps, managing multiple clusters, and enforcing policies across multiple clusters at scale.
* **Kustomize** - [Kustomize](https://kustomize.io/) traverses a Kubernetes manifest to add, remove or update configuration options without forking. 
* **Helm** - [Helm](https://helm.sh/) helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes deployments.
* **Ansible** - [Ansible](https://www.ansible.com/) Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. 
* **Operator Lifecycle Manager (OLM)** -The [Operator Lifecycle Manager (OLM)](https://docs.openshift.com/container-platform/4.7/operators/understanding/olm-what-operators-are.html) helps users install, update, and manage the lifecycle of all Operators and their associated services running across their clusters. 

## Event Driven Architecture
---
* **Domain Driven Design** - [Domain-driven](https://martinfowler.com/bliki/DomainDrivenDesign.html) design is the concept that the structure and language of software code should match the business domain.
* **Event Storming** - [Event storming](https://www.eventstorming.com/) is a workshop-based method to quickly find out what is happening in the domain of a software program.
* **Hexagonal Architecture** - The [hexagonal architecture](https://java-design-patterns.com/patterns/hexagonal/), or ports and adapters architecture, is an architectural pattern used in software design. 
* **Strategic Domain Driven Design with Context Mapping** - (Strategic Domain Driven Design with Context Mapping)[https://www.infoq.com/articles/ddd-contextmapping/]

## Demos
---
* [Deploy Quarkus Coffee Shop to multiple clusters using ACM]({% post_url 2021-05-13-acm-using-kustomize %})
* [Manage development teams using ACM]({% post_url 2021-03-08-mutlidevtem %})
* [Manage Cluster Policys using ACM](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-gitops/blob/master/acm-policys.md)
* [Quarkus Cafe Deployment on ACM using tekton pipelines](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-gitops/blob/master/tekton-demo-deployment.md)
* [Develop Helm Charts for OpenShift deployment]({% post_url 2021-02-09-helmchart %})
* [Create deployment Pipelines for microservices using Tekton]({% post_url 2021-05-13-tekton-pipelines %})