---
layout: page
title: CoffeeShop
permalink: /coffeeshop/
---

The application consists of the following microservices,:

* **Web** - the web front end (no way you saw that coming)
* **Counter** - coordinates events in the system
* **Barista** - makes drinks
* **Kitchen** - makes food
* **Inventory** - stores and restocks the inventory for the Barista and Kitchen microservices

The applications have 2 shared dependency:

* **Domain** - which contains the shared domain model
Shared dependencies are of course a bad idea in a Microservices Architecture so we'll talk about why this exists

And one optional dependency:

* **Testutils** - which contains optional classes that make testing easier

## Quarkus Coffeeshop Core Microservice
* [Quarkus Coffeeshop Core Microservice]({% post_url 2021-05-13-counter %})

## quarkus-cafe-web
[quarkus-cafe-web]({% post_url 2021-05-13-quarkus-cafe-web %})
> This service hosts the web front end and is the initial entry point for all orders. Orders are sent to a Kafka topic, where they are picked up by the Counter service
This services listens to another Kafka topic for updates and streams updates to the html page with server sent events


## quarkus-cafe-barista project
* [quarkus-cafe-barista]({% post_url 2021-05-13-quarkus-cafe-barista %})


## Kitchen Microservice
* [Kitchen Microservice]({% post_url 2021-05-13-quarkuscoffeeshop-kitchen %})

## quarkus-coffesshop-inventory project
* [quarkus-coffesshop-inventory]({% post_url 2021-05-13-quarkus-coffesshop-inventory %})

## quarkuscoffeeshop-customermocker project
* [quarkuscoffeeshop-customermocker]({% post_url 2021-05-13-quarkuscoffeeshop-customermocker %})

## customerloyalty project
* [quarkuscoffeeshop-customerloyalty]({% post_url 2021-05-13-customerloyalty %})


## Quarkuscoffeeshop Support
* [quarkus-coffesshop-inventory](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support)