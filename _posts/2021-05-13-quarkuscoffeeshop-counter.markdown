---
layout: post
title:  "quarkuscoffeeshop-counter"
date:   2021-05-13 12:00:00 -0500
categories: coffeeshop
---

# quarkuscoffeeshop-counter

## Running

Docker and a JDK are required to run locally

[quarkuscoffeeshop-support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support) contains a Docker Compose file that will spin up Kafka and PostgreSQL.  This will need to be started before the microservices

```
git clone https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support
cd quarkuscoffeeshop-support
docker compose up
```

[quarkuscoffeeshop-counter](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter) requires 4 environment variables to be set and can then Quarkus can be started in dev mode

```
export KAFKA_BOOTSTRAP_URLS=localhost:9092 \
PGSQL_URL="jdbc:postgresql://localhost:5432/coffeeshopdb?currentSchema=coffeeshop" \
PGSQL_USER="coffeeshopuser" \
PGSQL_PASS="redhat-21"

./mvnw clean compile quarkus:dev
```

## About

This microservices orchestrates the incoming and outgoing orders, persists orders in a database, and saves domain events in a database.  In a physical cafe the counter orchestrates the incoming orders.  So following Domain Driven Design's [ubiquitous language](https://martinfowler.com/bliki/UbiquitousLanguage.html) practice this microservice is named "counter." 

## Code
The package structure reflects Domain Driven Design principles and is organized using DDD principles with 2 primary packages, "domain" and "infrastructure."  The "domain" package contains sub-packages for "commands," "events," and "valueobjects."

Entities are persisted using the [Repository Pattern](https://martinfowler.com/eaaCatalog/repository.html)

## Domain

### [Order](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter/blob/master/src/main/java/io/quarkuscoffeeshop/counter/domain/Order.java)
The Order domain object contains all of the business logic for creating and updating orders.  It is also a [Hibernate Panache](https://quarkus.io/guides/hibernate-orm-panache) Entity.  Hibernate Panache is a major new feature of the [Hibernate](http://hibernate.org/) project that makes persistence extremely easy.

The Order's id field is a String and is assigned at creation from the web ui.

The creation and update methods return an [OrderEventResult](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter/blob/master/src/main/java/io/quarkuscoffeeshop/counter/domain/valueobjects/OrderEventResult.java) an object which encapsulates the Order instance and any associated value objects created by the business logic.

### [OrderRepository](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter/blob/master/src/main/java/io/quarkuscoffeeshop/counter/domain/OrderRepository.java)

OrderRepository is a [Hibernate Panache Repository](https://quarkus.io/guides/hibernate-orm-panache#solution-2-using-the-repository-pattern).  The default findById method is overridden to take a String.

## Infrastructure

### [OrderService](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter/blob/master/src/main/java/io/quarkuscoffeeshop/infrastructure/OrderService.java) 
The OrderService orchestrates order activity including persisting orders, sending messages to the Barista and Kitchen microservices, and sending messages to the web.

### [KafkaService](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-counter/blob/master/src/main/java/io/quarkuscoffeeshop/infrastructure/KafkaService.java) 
The KafkaService listens to the "orders-in" and "orders-up" topics and calls the appropriate methods on the OrderService.

### [OrderTicketDeserializer](), [PlaceOrderCommandDeserializer](), and [TicketUpdateDeserializer]()
These objects implement Quarkus' ObjectMapperDeserializer so no marshalling to and from JSON is required.  The excellent [Quarkus Guides](https://quarkus.io/guides/kafka#serializing-via-jackson) addresses this approach.