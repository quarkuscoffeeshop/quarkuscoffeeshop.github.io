---
layout: default
title: QuarkusCoffeeshop
permalink: /coffeeshop/
---

# Basics

This application can be run locally or deployed to Kubernetes/OpenShift.

Running the demo locally requires Docker and Java

# Running Locally

_NOTE:_ Docker and Java are required to run the demo locally

* Docker can be downloaded from <a href="https://www.docker.com/" target="_blank">docker.com</a>
* Java can be downloaded from several places.  Your authors recommend <a href="https://adoptopenjdk.net/" target="_blank">AdoptOpenJDK</a>

To run locally:
* clone the appropriate repos
* run Docker Compose which will start PostgreSQL and Kafka, both of which are required by the application and PGAdmin which can be used to view the database contents
* start the microserives

## Step 1: Clone the Repositories

To run the complete coffeeshop demo locally you will need to clone:
* [quarkuscoffeeshop-support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support)
* [quarkuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter)
* [quarkuscoffeeshop-barista](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-barista)
* [quarkuscoffeeshop-kitchen](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-kitchen)
* [quarkuscoffeeshop-web](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-web)

```shell
git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support.git
git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter.git
git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-barista.git
git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-kitchen.git
git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-web.git
```

## Step 2: Fire up Kafka and PostgreSQL (with Docker Compose)

[quarkuscoffeeshop-support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support) contains a Docker Compose file that will spin up Kafka and PostgreSQL.  This will need to be started before the microservices

From inside the quarkuscoffeeshop-support directory run:

```shell
docker compose up
```

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

You should be up and running!

# Running in OpenShift

The application can be deployed to OpenShift with Ansible.  Please see  <a class="page-link" href="/devops/">DevOps</a> for instructions.

There is no need to build the microservices locally to run them in OpenShift because they are already deployed to <a href="https://quay.io/organization/quarkuscoffeeshop" target="_blank" >Red Hat Quay</a>. 

# What's in the Demo

This demo models an individual coffeeshop where users can order drinks and food and view the status of their order.

The demo illustrates the general concepts of Event Driven Architecture, Domain Driven Design, and Event Sourcing.  The demo also specifically illustrates using Kafka with Quarkus and Debezium with Quarkus.

If you are running locally:
1.  Open <a href="http://localhost:8080" target="_blank" >http://localhost:8080</a>
2.  Choose a beverage and/or baked goods
3.  Watch as your order status is updated

## What happens behind the scenes

### Web Microservice

1. The web page posts your order as JSON to a REST endpoint
2. The REST endpoint creates a PlaceOrderCommand object and sends that object in JSON format to a Kafka topic named "orders-in"
3. The application listens to a Kafka topic, "web-updates," and when a message is received it is marshalled into JSON and sent to the web front end using server sent events

### Counter Microservice

1.  The counter microservice listens on the Kafka topic, "orders-in," and receives the order from the web
2.  The counter microservice creates value objects for the barista and kitchen microservices and sends them to the approrpriate Kafka topics
3.  The counter microservice persists the Order to the PostgreSQL database
4.  The counter microservice persists an OrderCreatedEvent 
4.  The counter microservice listes on the "orders-up" topic, and receives messages from the barista and kitchen microservices
5.  The counter microservices updates and persists the Order
6.  The counter microservice creates and persists an OrderUpdatedEvent and OrderCompletedEvent (when all of the orders' line items are fulfilled)
7.  The counter microservice creates a web update value object and places it on the "web-updates" Kafka topic

### Barista and Kitchen Microservices

1.  The barista and kitchen microservices listen to the "barista-in" and "kitchen-in" Kafka topics
2.  When an order comes in they apply the logic for making an order
3.  When the order is ready they send an update to the "orders-up" Kafka topic


