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
>This service orchestrates and persists order related events
Quarkus Coffeeshop Core Microservice]({% post_url 2021-05-13-quarkuscoffeeshop-counter %})

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
* [quarkuscoffeeshop-customerloyalty]({% post_url 2021-05-13-customerloyalty %})

## Quarkuscoffeeshop Support
>This repo contains support files for local development
[Quarkuscoffeeshop Support](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-support)