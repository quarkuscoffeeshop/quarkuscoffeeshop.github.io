---
layout: page
title: QuarkusCoffeeshop
permalink: /coffeeshop/
---

This application can be run locally or deployed to Kubernetes/OpenShift.

Running the demo locally requires Docker and Java

# Running Locally

_NOTE:_ Docker and Java are required to run the demo locally

The below links opens in a new window/tab

* Docker can be downloaded from <a href="https://www.docker.com/" target="_blank">docker.com</a>
* Java can be downloaded from several places.  Your authors recommend <a href="https://adoptopenjdk.net/" target="_blank">AdoptOpenJDK</a>

## Step 1: Clone the Repositories

To run the complete coffeeshop demo locally you will need to clone:
* [quarkuscoffeeshop-support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support)
* [quarkuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter)
* [quarkuscoffeeshop-barista](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-barista)
* [quarkuscoffeeshop-kitchen](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-kitchen)
* [quarkuscoffeeshop-web](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-web)

## Step 2: Fire up Kafka and PostgreSQL (with Docker Compose)

[quarkuscoffeeshop-support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support) contains a Docker Compose file that will spin up Kafka and PostgreSQL.  This will need to be started before the microservices

## Step 3: Set the Environment Variables

The microservices all require environment variables to be set

[quarkuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter)
```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \
PGSQL_URL="jdbc:postgresql://localhost:5432/coffeeshopdb?currentSchema=coffeeshop" \
PGSQL_USER="coffeeshopuser" \
PGSQL_PASS="redhat-21"
```

[quarkuscoffeeshop-barista](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-barista) and [quarkuscoffeeshop-kitchen](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-kitchen)
```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \
```

[quarkuscoffeeshop-web](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-web)
```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \ 
STREAM_URL=http://localhost:8080/dashboard/stream \
CORS_ORIGINS=http://localhost:8080 \
STORE_ID=CHARLOTTE
```
## Step 4: Start the Microservices

Once the environment variables are set the services can be started with:
```

./mvnw clean compile quarkus:dev
```

# Running in OpenShift

The application can be deployed to OpenShift with Ansible.  Please see  <a class="page-link" href="/devops/">DevOps</a> for instructions.

There is no need to build the microservices locally to run them in OpenShift because they are already deployed to <a href="https://quay.io/organization/quarkuscoffeeshop" target="_blank" >Red Hat Quay</a>. 

# Overview

This demo models an individual coffeeshop and contains a web frontend where users can order drinks and food.  The application consists of the following microservices:

### [quarkuscoffeeshop-web](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-web)

The web front end (no way you saw that coming.) This service hosts the web front end and is the initial entry point for all orders. Orders are sent to a Kafka topic, where they are picked up by the Counter service

The web service listens to another Kafka topic for updates to orders and then streams the updates to the html page using server sent events  
[quarkuscoffeeshop-web]({% post_url 2021-05-13-quarkuscoffeeshop-web %})

## [quar#kuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter)

## [quarkuscoffeeshop-barista]({% post_url 2021-05-13-quarkuscoffeeshop-barista %})
The barista services consumes "OrderIn" events, applies the business logic for making the beverage, and produces, "OrderUp" events. The terms "OrderIn" and "OrderUp" are part of our ubiquitous language  



Supporting files can be found in the quarkuscoffeeshop-support project: https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support 






## Kitchen Microservice
>The kitchen services consumes "OrderIn" events, applies the business logic for making the item, and produces, "OrderUp" events. The terms "OrderIn" and "OrderUp" are part of our ubiquitous language  
[Kitchen Microservice]({% post_url 2021-05-13-quarkuscoffeeshop-kitchen %})

## quarkuscoffesshop-inventory project
[quarkuscoffeeshop-inventory]({% post_url 2021-05-13-quarkuscoffeeshop-inventory %})

## quarkuscoffeeshop-customermocker project
[quarkuscoffeeshop-customermocker]({% post_url 2021-05-13-quarkuscoffeeshop-customermocker %})

## customerloyalty project
[quarkuscoffeeshop-customerloyalty]({% post_url 2021-05-13-customerloyalty %})

## Quarkuscoffeeshop Support
>This repo contains support files for local development  
[Quarkuscoffeeshop Support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support)
