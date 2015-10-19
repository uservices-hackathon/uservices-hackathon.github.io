WJUG 2015 Microservices
=====================

Welcome to the WJUG 2015 Microservices!

Here you find all information required for you to work during the WJUG 2015 Microservices presentation.

# Table of contents

- [Documentation](#documentation)
- [How to run my microservice locally](#how-to-run-my-microservice-locally)
- [What's my service's port?](#ports)
- [Where are all the tools?](#tools)
- [Working example of the whole setup](#working)

---

# <a name="documentation"/></a> Documentation

## Good to do before the workshop

- Clone and build boot-microservice so that all the libraries get downloaded. 
    - [Link to the boot-microservice repository](https://github.com/4finance/boot-microservice) which will be the base for the services that we will build 
    - Execute command `git clone git@github.com:4finance/boot-microservice.git && cd boot-microservice && ./gradlew clean build` to clone and built the application 
- [Read about Microservices](http://martinfowler.com/articles/microservices.html)
- [Watch a video about Microservices](https://www.youtube.com/watch?v=wgdBVIX9ifA)
- [Read about 12 Factor App](http://12factor.net/)
- [Read about Micro-Infra-Spring](https://github.com/4finance/micro-infra-spring/wiki)
- [Watch a video about Micro-Infra Spring](https://www.youtube.com/watch?v=D6V49K_Yb8g)
- [Read about Consumer Driven Contracts] (http://martinfowler.com/articles/consumerDrivenContracts.html)
- [Watch a video about Consumer Driven Contracts](https://vimeo.com/130779882)

### If you don't know anything about Spring / Guava / Concurrency

- [Read a getting started to Spring Boot](https://spring.io/guides/gs/spring-boot/)
- [Read about ListenableFuture](https://code.google.com/p/guava-libraries/wiki/ListenableFutureExplained)
- [Definitive guide to CompletableFuture](http://www.nurkiewicz.com/2013/05/java-8-definitive-guide-to.html)

## Presentations and introductions

- [Codepot presentation](https://docs.google.com/presentation/d/1ZSMaZJrvurvH3-EKuI2DXifKenPVp2xq80uzJvr7yzs/edit?usp=sharing)
- [Micro-Infra spring fast introduction - online](https://docs.google.com/presentation/d/1xbdOWYvuGKnp-_1wGz-bZTobEbCePRr062YUnx4jTQg/edit?usp=sharing)
- [Micro-Infra spring fast introduction - pdf](resources/Microservices_Codepot.pdf)

## Additional links

- [Ansible scripts used for provisioning of the Workshop setup](https://github.com/microservice-hackathon/infrastructure)
- [Jenkins-DSL code to setup Jenkins jobs for the Workshop](https://github.com/microservice-hackathon/jenkins/tree/2015-08-codepot-offline)
- [Micro-Infra-Spring codebase](https://github.com/4finance/micro-infra-spring)

---

# <a href="how-to-run-my-microservice-locally"></a> How to run my microservice locally

To start it's enough to run the following Gradle Command

```
./gradlew bootRun -Dspring.profiles.active=DEV -DAPP_ENV=dev
```

#  <a name="working"/></a> Working example of the whole setup

[Link to the Github organization with proper setup](https://github.com/uservices-hackathon)


