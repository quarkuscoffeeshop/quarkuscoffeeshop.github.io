
---
layout: post
title:  "homeoffice-backend project"
date:   2021-05-13 12:00:00 -0500
categories: homeoffice
---

# homeoffice-backend project

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .


## Clone repo 
        git clone https://github.com/quarkuscoffeeshop/homeoffice-backend.git
        cd homeoffice-backend

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```
./mvnw quarkus:dev
```

## Packaging and running the application

The application can be packaged using `./mvnw package`.
It produces the `homeoffice-backend-1.0-SNAPSHOT-runner.jar` file in the `/target` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/lib` directory.

The application is now runnable using `java -jar target/homeoffice-backend-1.0-SNAPSHOT-runner.jar`.

## Creating a native executable

You can create a native executable using: `./mvnw package -Pnative`.

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: `./mvnw package -Pnative -Dquarkus.native.container-build=true`.

You can then execute your native executable with: `./target/homeoffice-backend-1.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/building-native-image.


## Links
[homeoffice-backend project repo](https://github.com/quarkuscoffeeshop/homeoffice-backend)