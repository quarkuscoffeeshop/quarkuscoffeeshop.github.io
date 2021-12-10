---
layout: page
title: QuarkusCoffeeshop
permalink: /coffeeshop/
---

This application can be run locally by cloning the individual repositories or deployed to Kubernetes/OpenShift.

# Running Locally

_NOTE:_ Docker is required to run the demo locally

To run the complete coffeeshop demo locally you will need to clone:
* quarkuscoffeeshop-support
* quarkuscoffeeshop-counter
* quarkuscoffeeshop-barista
* quarkuscoffeeshop-kitchen
* quarkuscoffeeshop-web

quarkuscoffeeshop-support contains a Docker Compose file that will spin up Kafka and PostgreSQL.  This will need to be started before the microservices

The microservices all require environment variables to be set

quarkuscoffeeshop-counter
```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \
PGSQL_URL="jdbc:postgresql://localhost:5432/coffeeshopdb?currentSchema=coffeeshop" \
PGSQL_USER="coffeeshopuser" \
PGSQL_PASS="redhat-21"
```

quarkuscoffeeshop-barista and quarkuscoffeeshop-kitchen
```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \
```

quarkuscoffeeshop-web
```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \ 
STREAM_URL=http://localhost:8080/dashboard/stream \
CORS_ORIGINS=http://localhost:8080 \
STORE_ID=CHARLOTTE
```

Once the environment variables are set the services can be started with:
```

./mvnw clean compile quarkus:dev
```

# Running in OpenShift

The application can be deployed to OpenShift with Ansible.  Please see  <a class="page-link" href="/devops/">DevOps</a> for instructions.

The application consists of the following microservices,:

* **Web** - the web front end (no way you saw that coming)
* **Counter** - coordinates events in the system - https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter
* **Barista** - makes drinks - https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-barista
* **Kitchen** - makes food - https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-kitchen
* **Inventory** - stores and restocks the inventory for the Barista and Kitchen microservices - https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-inventory

The current versions are:

* Web 
* Counter 5.1.1
* Barista  5.1.0
* Kitchen 5.1.0
* Inventory

Supporting files can be found in the quarkuscoffeeshop-support project: https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support 

## [quarkuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter)
>This service orchestrates and persists order related events  
[Quarkus Coffeeshop Counter Microservice]({% post_url 2021-05-13-quarkuscoffeeshop-counter %})

## quarkuscoffeeshop-web
> This service hosts the web front end and is the initial entry point for all orders. Orders are sent to a Kafka topic, where they are picked up by the Counter service
This services listens to another Kafka topic for updates and streams updates to the html page with server sent events  
[quarkuscoffeeshop-web]({% post_url 2021-05-13-quarkuscoffeeshop-web %})


## quarkuscoffeeshop-barista project
>The barista services consumes "OrderIn" events, applies the business logic for making the beverage, and produces, "OrderUp" events. The terms "OrderIn" and "OrderUp" are part of our ubiquitous language  
[quarkuscoffeeshop-barista]({% post_url 2021-05-13-quarkuscoffeeshop-barista %})


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
