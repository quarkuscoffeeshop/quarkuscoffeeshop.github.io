---
layout: post
title:  "quarkuscoffeeshop-homeoffice-ui"
date:   2021-05-13 12:00:00 -0500
categories: homeoffice
---

# homeoffice-backend project

## Clone repo 
        git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-homeoffice-ui.git
        cd quarkuscoffeeshop-homeoffice-ui

## Local Development
        npm install
        npm run start:dev

You will need the homeoffice-backend running, which also depends on a postgresql db


## Full Stack Deployment on OpenShift

        oc new-project quarkuscoffeeshop-homeoffice

        oc new-app \
        -n quarkuscoffeeshop-homeoffice \
        --name postgres \
        --template="openshift/postgresql-persistent" \
        -e POSTGRESQL_USER=coffeeshopuser \
        -e POSTGRESQL_PASSWORD=redhat-20 \
        -e POSTGRESQL_DATABASE=coffeeshopdb


        oc new-app \
        -n quarkuscoffeeshop-homeoffice \
        --name homeoffice-backend quay.io/quarkus/ubi-quarkus-native-s2i:20.3.0-java11~https://github.com/quarkuscoffeeshop/homeoffice-backend.git \
        -e POSTGRESQL_JDBC_URL="jdbc:postgresql://postgresql:5432/coffeeshopdb?currentSchema=coffeeshop" \
        -e POSTGRESQL_USER=coffeeshopuser \
        -e POSTGRESQL_PASSWORD=redhat-20

        oc expose svc/homeoffice-backend

        oc new-app \
        -n quarkuscoffeeshop-homeoffice \
        --name=homeoffice-ui nodejs:latest~https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-homeoffice-ui.git \
        --build-env=REACT_APP_GRAPHQL_ENDPOINT=http://$(oc get routes -o json | jq -r '.items[0].spec.host' | grep homeoffice-backend)/graphql

        oc expose svc/homeoffice-ui

        oc annotate deployment -l app=homeoffice-ui app.openshift.io/connects-to='["homeoffice-backend"]'

        oc annotate deployment -l app=homeoffice-backend app.openshift.io/connects-to='["postgres"]'

        oc label dc -l app=postgres app.kubernetes.io/name=postgresql

        oc label deployment -l app=homeoffice-backend app.kubernetes.io/name=quarkus

        oc label deployment -l app=homeoffice-ui app.kubernetes.io/name=nodejs

## Links
[quarkuscoffeeshop-homeoffice-ui](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-homeoffice-ui)