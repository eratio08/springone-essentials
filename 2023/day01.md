# SpringOne Essentials 2023

@starbuxman & @dashaun

## Elevate the Developer Experience

(Bunch of short pep talks)

Some advertisement talks in the beginning.
VMWare Tanzu plarform.

Talking about how important Platform Teams are.
Spring seems to be some famous choice.
Microsoft now support spring boot cloud as a first class citizen.

Spring Framework v6 and Boot v3 are available.
New baseline is jdk 17+ and jakarta 9+.
Soon jdk 21 will be supported.
The new jakarta namespace changes, are quite a disruptive change and will require code changes everywhere.
Two generations Jakarte EE9 and EE10.
An early update to spring 6 and boot 3 is recommend strongly!

Cloud deployments are concerned with startup time and mem footprint.
Spring AOT processing is one solution to move steps from runtime to compile time.
Feels like Micronaut and we should check this out.
AOT processed spring apps can be turned into native images.
Project Leyden, spec fpr well-defined static image variants (from openJDK initiative).
Project CRaC (Coordinated Restore at Checkpoint) warmed-up HotSpot JVM snapshot. (also openJDK project)
Virtual threads with project loom.
Virtual Threads are attached to a platform threads, when ever it blocks it's detached from the carrier thread.
It's re-attached when the processing progressed.
JDK 19 in preview, 2nd preview jdk 20, maybe stable in jdk 21. 
In spring v6 Http Interface, RFC 780x standard http error response, micrometer based observability.
Q4 2023 6.1 and boot 3.1.
Spring Academy, learning plarform for spring stack (free & paid).

Native Java support for spring boot (thomaswue & s).
Spring Boot 3 support native images natively with graal vm.
Libs with native supported are listed on the Graal website.
Regular approach today it put spring into a container (javac + deployment).
With native images 2 new steps spring AOT & native image compiling to get a native image.
Instant startups, now warmup, low resource usage, reduced surface for attacks, compact packing.
It not a silver bullet.
There are downsides.
Recommended: low memory & cpu, scale to zero, serverless.
Assess: Microservices, backoffice, container images distribution, K8s
JVM Better: very frequent deployments, hight traffic website, agent-based obervability, unsupported dependencies, big monolith, huge mem & cpu.
Graal is working on improving speak performance & throughput.

How to get there?
Migrate to jdk 17.
Upgrade to Spring Boot 3.
Ensure native compat of dependencies (check Graal VM metadata registry).
Build native image.
Deploy native image.

VMWare offers support for native.

Java related parts of GraalVM got donated to the OpenJDK Foundation.

Demo Time: sGriemhild & sdeleuze.
Native image deployment in azure cloud with spring boot v3.
Will use scale to zero.
Azure has some clicky-bunti interface to start up.
Builds the image in the cloud.
Deploy the image to the spring enterprise cluster.
Migrate to graal: spring starter in intellij with graalvm.
Adds application properties class and adds a new controller.
App register reflection annotations to allow some reflection in native.
Buildpack to create image.
They show how some images are uploaded to the azure object store after a http request to the api.
Azure spring apps to create a deployment with a scale to zero setup.
Azure supports observability for the native images.
14s (jvm) some milliseconds with native.

Microsoft enables golden path for spring apps.
Golden path to production is the buzz word here.
Azure Spring App Enterprise.
App accelerator and live view.
New pricing plans with scale to zero, event driven up/down scaling.
FedEx uses spring and azure.
With package delivery prediction system.
FedEx dataworks: use data to optimize supply chains.
Global delivery prediction platform.
3 tier machine learning model with spring.
Huge data load in the fedex system.

<image>

Goal was to improve visibility to the customers (shipper and recipient).

The data driven digital journey.
25000 tps.
Why spring?
Spring makes app2app communication and security easy.
Mainly also because the core system is java.

Spring Security: sjohrn & joe_grandja
Spring Security builds an authorization server.
OIDC, Saml, LDAP... SSO, Identity Federation.
Quite simple to setup an OIDC auth-server.
Easy to implement federated login.

Tanzu: missquirkygal
Developer XP: Pattern-based-development, self-service & collaboration, deploy often and frequently, reduce toil.
No to tickets, to yaml.
Tanzu: IDE Integration (VSCode), App templates, API discovery & management, logging & insights, security enhancements.
Buzzword bingo.
RabbitMQ as a service at VMWare in Beta.
GemFire 10 in Beta (Fulltext search).
VMWare application catalog.

Tanzu.tv weekly streaming about tanzu.

## Interface Clients in Spring

What is a declarative client?
We could use raw rest template.
We could declare an interface with methods that represent the endpoints.
The new way would be to have an annotated interface and generate the rest.
It like the counterpart to a controller declaration.
There is a similar approach already existing: OpenFeign/Feign.
Spring Cloud OpenFeign project.
Uses feign contracts to integrate with spring cloud.
Spring MVC annotations support.
Integrated with spring cloud projects.
Tracing is supported.
Spring Cloud Suqare to support reactive web client support.
RSocket interface client is also available now.

How is the programming model?
Map to a single request.
Avoid server-ism.
So `@RequestMapping` on the server should handle any request.
When using it at the client HttpMethod, `@RequestHeader`, `@RequestParam`, `@RequestBody` and `@RequestAtribute` are available.
Also ResponseEntity, HttpHeaders & Body.
So server side programming model that make no sense are not available.
New `@HttpExchange` with url, method, contentType accept.

## Modern Persistence with Spring Data 3

Baseline: java 17, Spring 6, Jakarta EE9.
Switched to `CompletableFuture` from `ListenableFuture`.
Still there, but placed in a legacy package.
RxJava1 and 2 support support removed, switch to rxjava 3 or reactor.
Javax is gone, so use jakarta namespace.

<image>

There is a spring migrator to handle this as well.
Extended support period for the 2.x line Nov 2023.
Check the support tap on the spring page.
No support for Apache Geode anymore.
New Stuff!
Composable repository.
The repository hierarchy was re-designed.
`PagingAndSortingReposiroty`, with paging and sorting, currently also has crud methods.
In 3 paging and sorting does not have any crud methods, crud needs to be added explicitly.
New interfaces: `ListCrudRepository` and others.
GraalVM: support for native images.
In the JVM all is done at runtime.
In Graal it has to be done at build time.
Observability: is now backed into spring data (using micrometer).
Here: used zipkin to drill down to the query.
Mongodb observability not supported natively yet, can be setup manually.
QueryRewriter now allows to rewrite query or append filter etc (e.g. tenant id).
SpEL supported in `@Query`.
Call backs to hook into the lifecycle.
Lifecycle event can now be turned off.
JDBC statements are now batched in the driver, for safes and deletes.
MongoDB allows aggregation framework of mongodb in queries.
Future: client side encryption.

## End-to-End Machine and Deep Learning with MLFlow and Spring

MLOps Problem: Mixing DevOps with Data/ML.
There is a gap between dev ops and ml ops.
Only 26% of ml model make it to production.
Why?
ML oriented workload are more dynamic.
Experimental by design.
Multiple iterations, possibly non-deterministic.
Runs must be tacked analyzed & audited.
Monitoring is required.
How to track?
DRIFTS, SKEWS and LAGS.
Data drift: a good model is able to detect this.
Model drift/ covariant shift.
Training/Serving skews: training data does not match production data.
Feature lag and label lag.
Features or labels will only be fully available way later in the data.
More monitoring needs to be in place to detect these.
Scale on demand based on the model e.g. huge models that do not fit a single node.
General lifecycle management of ml models.
Continuous Integration, Deployment, Training.
Management: Code, Models, Configuration.
Scoping, Data Ingestions, Feature Engineering, Model (re)Training, Model Selection, Model Deployment, Model Monitoring.
