Microservice Hackathon
=====================

Welcome to the Microservice Hackathon!

Here you find all information required for you to work during the Microservice Hackathon.

# Table of contents

- [Documentation](#documentation)
- [How to run my microservice locally](#how-to-run-my-microservice-locally)
- [What's my service's port?](#ports)
- [Where are all the tools?](#tools)
- [Working example of the whole setup](#working)
- [Useful snippets / info](#useful-snippets-info)
    - [Spring Rest Controller example](#spring-rest-controller)
    - [Service Rest Client example](#service-rest-client)
    - [Metrics setting example](#metrics)
    - [Seyren setup example](#seyren)

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

# <a name="useful-snippets-info"/></a> Useful snippets / info

## <a name="spring-rest-controller"/></a> Spring Rest Controller example

Example of a `Controller` with an endpoint at URL `/video` that accepts the `content-type` header equal to `application/vnd.some.service.v1+json`
and produces a `JSON` as output. You have to call it via `POST` HTTP method

```java
@RestController
@RequestMapping(value = "/video", consumes = "application/vnd.some.service.v1+json", produces = MediaType.APPLICATION_JSON_VALUE)
public class SomeController {

    private final VideoService videoService;

    @Autowired
    public SomeController(VideoService videoService) {
        this.videoService = videoService;
    }

    @RequestMapping(method = RequestMethod.POST)
    public Videos doSomeFancyStuff(@RequestBody SomeRequestBody someRequestBody) {
        return videoService.doSomethingAwesome(someRequestBody);
    }
}
```

## <a name="service-rest-client"/></a> Service Rest Client example

Example of a component registered via `@Component` (you should use `@Configuration` to register beans) that uses
 `ServiceRestClient` to call other components.

```java
import com.netflix.hystrix.HystrixCommand;
import com.nurkiewicz.asyncretry.RetryExecutor;
import com.ofg.infrastructure.web.resttemplate.fluent.ServiceRestClient;
import com.netflix.hystrix.HystrixCommand;

import static com.netflix.hystrix.HystrixCommandGroupKey.Factory.asKey;

@Component
public class VideoService {

    private final ServiceRestClient serviceRestClient;
    private final RetryExecutor retryExecutor;

    @Autowired
    public VideoService(ServiceRestClient serviceRestClient, RetryExecutor retryExecutor) {
        this.serviceRestClient = serviceRestClient;
        this.retryExecutor = retryExecutor;
    }

    public ListenableFuture<Videos> doSomethingAwesome(Ingredients ingredients) {
        return serviceRestClient.forService("someAliasFromMicroserviceDescriptor")
                .retryUsing(retryExecutor)
                .post()
                .withCircuitBreaker(HystrixCommand.Setter.withGroupKey(asKey("hystrix_group")))
                .onUrl("/someUrlToWhichYouWantToSendARequest")
                .body(ingredients)
                .withHeaders().contentType("application/vnd.some.other.service.v1+json")
                .andExecuteFor()
                .anObject()
                .ofTypeAsync(Videos.class)
    }
}
```


## <a name="metrics"/></a> Metrics setup example

```java
@Component
class IngredientsAggregator {

    private final SomeService someService; 
    private final Meter someMeter;

    @Autowired
    IngredientsAggregator(MetricRegistry metricRegistry,  SomeService someService) {
        this.someService = someService;
        this.someMeter = metricRegistry.meter("name.of.a.meter.metric");
        setupMeters(metricRegistry);
    }

    private void setupMeters(MetricRegistry metricRegistry) {
        metricRegistry.register("name.of.the.gauge.metric", (Gauge<Integer>) () -> someService.getValueForGauge());
    }

    public void doSomethingMeaningful(long sample) {
        // do something and mark the metric
        someMeter.mark(sample);
        // do something else
    }

}

```

## How to take current CorrelationID

Use the following snippet

```
import com.ofg.infrastructure.correlationid.CorrelationIdHolder;


CorrelationIdHolder.get();

```

## <a name="seyren"/></a> How to configure alerting in Seyren

In order to add alerting in Seyren you have to first:

- Add hosts entries as presented in this doc
- Go to `http://seyren.codepot`

Now do the following actions:

### Go to checks

Click the _Checks_ button to the top: 

![alt text](images/seyren/seyren_checks.png "Seyren Checks")

### Create a new check

In the checks site you will have a list of already created checks.

Click the _Create check_ button to the top right: 

![alt text](images/seyren/seyren_create_check.png "Create Seyren Check")

### Fill out the dialog

![alt text](images/seyren/seyren_create_check_dialog.png "Create Seyren Check Dialog")

Here you have a description of the fields.

- Name
    - Name of the alert in format: [REALM][APP] Alert name
    - Example: `[RED][BUTELKATR] Number of beer bottles` 
- Description 
    - Description of the alert
    - Example: `Number of beer bottles`  
- Target 
    - Function from Graphite basing on a Graphite metric. Example shows a Graphite function keepLastValue applied to a Graphite metric. Note that you might get NULL values from Graphite so keepLastValue is a function that in that case will help you pick the last value that was present in Graphite.
    - Example: `keepLastValue(.*.red.butelkatr.bottles.meter.m15_rate)`
    - Read more here about Graphite functions: [Graphite functions](https://graphite.readthedocs.org/en/0.9.10/functions.html)
- Warn level
    - What is the threshold that when the metric goes BELOW will set the level to WARN 
    - Example: `100`  
- Error level
    - What is the threshold that when the metric goes BELOW will set the level to ERROR
    - Example: `10`
- Enabled 
    - Enables the metric 
    - Can be (on) or (off) 
- Allow no data 
    - Assumes that if there are null values then it's not a problem 
    - Can be (on) or (off)

### Add subscription

Once you've added a metric most likely you want to get notified if some threshold has been reached. To do that you have to add a subscription:

![alt text](images/seyren/seyren_add_subscription.png "Create Subscription for an alert")

### Fill out the subscription dialog

Pick `Slack` as _Type_ and type in the _channel name with a hash at the beginning`. e.g. `#channel` 

![alt text](images/seyren/seyren_create_subscription.png "Fill out the subscription dialog")

## How to run in debug mode with stubs from nexus

application.yaml

```yaml
stubrunner:
  #skip-local-repo: false
  #work-offline: true
  use-microservice-definitions: true
  stubs:
    repository.root: "http://52.16.215.68:8081/nexus/content/repositories/releases/"
```

### How to start in dev mode

To start it's enough to run the following Gradle Command

```
./gradlew bootRun -Dspring.profiles.active=DEV -DAPP_ENV=dev
```

---

#  <a name="working"/></a> Working example of the whole setup

[Link to the Github organization with proper setup](https://github.com/uservices-hackathon)


