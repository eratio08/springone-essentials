# SpringOne Essentials Day 2

## Spring Framework 6 Infrastructure  Themes

Joergen Hoeller, VMWare

Minimum requirements is jdk 17.
Spring 6.1 will support both lts 17 & 21.
Why 17?
Text blocks, switch expressions, collections factories, etc.
Records, sealed classes.
Module introspection, module path scanning.
These changes empower frameworks & applications.
They are focused on jdk 19 right now.
Virtual threads and record pattern.
Consider upgrading jdk with spring framework together.
Other baseline tipoc: Jakarta EE9.
The namespaces changed is simple in the codebase.
The change is hard if libs do not change.
Value preposition: These API will finally be maintained again.

Tomcat 10.0(5.0) -> 10.1(6.0). 
Jetty 11(5.0) -> 12(6.0).
Undertow 2.3 based on Servlet API 6.0.
Hibernate ORM 6.1 vased on JPA 3.0/3.1.
Hibernate Validator 7/8 based on Bean Validation 3.0.
Note: API upgrades not coupled to major provider version anymore.

Jakarta EE10 is preferred by spring.
6.0 has atuo-detection for EE10 APIs at runtime.
Same for Spring Boot 10.
Jetty 12 is not ready yet (implicit downgrade to 5).
Servlet mocks are Servlet 6.0 based.

Hibernate 5.6+ with 6.1 recommended.
Only supported through JPA API.
Netty 4.1.x, early support for Netty 5, but not final.
Jackson 2.14 supported.

The new baseline should prepare for the future.

Ahead-of-Time Compiling.
Reduces startup time and memory footprint in production.
Minimized reflection.
Optional for optimized JVM deployments.
Precondition for GraaVM native executables.
AOT: extra build setup required.

GraalVM native images, de-facto standard for native executable.
String closed-world-assumption, no runtime adaption.
You need to know all you dependencies and they need to be graal compatible.
Very long build time for actual native code.
It's a different mode for deployment with string benefits but also string limitations.
The ecosystem is still in development and better tooling will evolve.

Project Leyden: OpenJDK aims tp introduces well-defined static images, not only GraalVM.
E.g. custom hotspot-based runtimes images.
Ongoing effort.
 
Project CRaC by Azul.
Bootstraping from a warmed-up HotSpot JVM snapshot.
With Spring: a checkpoint after application context refresh.
You skip all the warmup and shorten startup time.
It's a regular JVM setup.
AWS Lambdas support this with SnapStart.
A potentially simple solution.

Evolving in 2023.
GraalVM 23+ aligning its releases schedule with open jdk.
Project Galahad: GraapVM technologies merging into open jdk.
Project Leyden: specification for static image variants.
Potential CRaC support of spring boot.

Virtual Threads.
Lightweight threading model in the JVM.
Improves scalability of imperative programming.
It's implemented as a variant of `java.lang.Thread`.
Some alignment need to be done e.g. no synchronize blocks aground IO.
Spring is already preparing for it.

Virtual Threads for Web Applications.
Servlet callbacks methods based on InputStream/OutputStream.
Loom-ready database drivers. 
Ideally no changes in the main code base.
Significant scalability benefits expected for database-driven web apps.
In scheduling/messaging.
Useful for any case of explicit thread usages in java.
JMS messages listener containers, `@Scheduled` handler methods.
Many of these trigger I/O operations hence the benefits.
Pure CPU bound handler no benefits are to be expected.
If the are any blocking operations improvements will be seen.

Virtual Thread vs Reactive Programming.
It's a different scalability mechanism.
Traditional imperative programming style.
Empowering spring mvc to reach maximum scalability.
WebFlux is for stream-based processing, not for scalability reasons any more.

Embrace virtual threads.
Higher scalability, smaller footprint.
Best possible use of available CPU resources.
Virtual thread are supported by GraalVM images as well.
The community has high expectations.

Spring Framework 6 is foundation for Spring Boot 3.0/3.1.
We should upgrade to be prepared for future features of the JVM and the Spring Framework.

## Observability of Your Application

Jonatan Ivanow, Marcin G., Timmy Ludwig

They demo some app that has an error.
All apps in the demo are spring boot apps.
They show some grafana boars.
How to find the reason for the failure?
They check for traces of the service that have an error.
In the traces we can see the call layers.
Next step is to check the correlated logs.
(Did not know grafan can do that.)
It even shows the SQL query.
We see the span of the query and see the result set.
Grafana has a service graph.
From the traces we go to metrics to get more insights.
e.g. Throughput or latency.
We saw some data is missing form the DB.
Using postman the data is inserted.
Checking the dashboard if the error rate goes down.
A new problem occurs, the latency has increased.
Check metrics of the process and the jvm.
Heap looks fine.
GC looks good.
Threads and Buffer looks fine.
Http shows latency issue.
The service has not db but uses downstream services.
Checking these services.
One service does not show any http latency.
This one is not the offender.
The other service show http latency.
The db access seems to be slow.
Checking if the db has the latency.
It has.
We need to see a trace.
The use exemplars(?) With have a trace id.
Now traces before and after the latency spikes are compared.
So comparing fast and slow request traces.
The db is the offender is this case.
Possible solution add caching.

This all was possible because logs and traces where correlated with traces identifiers.
Same for metrics and traces.
Spring portfolio allow micrometer observation. 
`jdbc-observations`, OpenFeign, etc also support this.

Whets new in micrometer 1.9.0.
Supports OTLP Registry (OpenTelemetry).
HighCardinalityTagsDetector.
Exemplars (Prometheus only).

In 1.10: Micrometer tracing, distributed tracing.
Micrometer docs generator: generate documentation for you instrumentations.
Micrometer Context propagation: solves cross cutting concerns.
Observation API in micrometer-core.

The API allows to start `Observation`.
Observations should know about exceptions and need to be stopped.
The `ObservationRegistry` get handlers attached that allow the correlation.
So additional behavior is attached here.
Observations low/high cardinally key values can be configured.
`@Observed` annotation to do observation with AOP.

## Azure Spring Apps: The Easy Way to Run Your Apps

TAP - Tanzul Application Platform.
What a developer platform?
Empowering a developer with self service APIs.
Book: The New Kingmakers.

![Developer Platform](images/devplatform.png)

What do developers need.
Self driven autonomy.
Platform for developers: application-centric abstraction.
SLAs.
Idealy `git push` should be enough.
Feedback is also very important e.g. Latency.
Consolidate every thing into a view of status, logs, metrics, traces, alerts.
Information Management.
APIs docs, ownership, dependencies, resources...
Enablement.

Make the right thing the obvious thing.
*Backstage* is a good choice for this.
It part of CNCF. 
TAP also utilized Backstage.

Demo time.

Tilt for K8s.

## AMA on Tazu Application Platform

Easy way to run Spring Apps on Azure.
This is an advert.
