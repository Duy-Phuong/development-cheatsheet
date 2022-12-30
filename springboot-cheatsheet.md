# pom.xml

https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/4/ch04lvl1sec31/coding-the-spring-boot-microservice

The following table shows some of the important starter projects provided by Spring Boot we will be using:

| **Project**                    | **Description**                                                                                                                                     |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| spring-boot-starter            | Base starter for Spring Boot applications. Provides support for auto-configuration and logging.                                                     |
| spring-boot-starter-web        | Starter project for building Spring MVC based web applications or RESTful applications. This uses Tomcat as the default embedded servlet container. |
| spring-boot-starter-data-jpa   | Provides support for Spring Data JPA. Default implementation is Hibernate.                                                                          |
| spring-boot-starter-validation | Provides support for Java Bean Validation API. Default implementation is Hibernate Validator.                                                       |
| spring-boot-starter-test       | Provides support for various unit testing frameworks, such as JUnit, Mockito, and Hamcrest matchers                                                 |

There are a lot more projects, which can be useful for you. We are not going to use them, but let's look at what else is available:

| spring-boot-starter-web-services | Starter project for developing XML based web services                                                                                                                                                 |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| spring-boot-starter-activemq     | Supports message based communication using JMS on ActiveMQ                                                                                                                                            |
| spring-boot-starter-integration  | Supports Spring Integration, framework that provides implementations for Enterprise Integration Patterns                                                                                              |
| spring-boot-starter-jdbc         | Provides support for using Spring JDBC. Configures a Tomcat JDBC connection pool by default.                                                                                                          |
| spring-boot-starter-hateoas      | HATEOAS stands for Hypermedia as the Engine of Application State. RESTful services that use HATEOAS return links to additional resources that are related to the current context in addition to data. |
| spring-boot-starter-jersey       | JAX-RS is the Java EE standard for developing REST APIs. Jersey is the default implementation. This starter project provides support for building JAX-RS based REST APIs.                             |
| spring-boot-starter-websocket    | HTTP is stateless. Web sockets allow maintaining connection between server and browser. This starter project provides support for Spring WebSockets.                                                  |
| spring-boot-starter-aop          | Provides support for Aspect oriented programming. Also provides support for AspectJ for advanced Aspect oriented programming.                                                                         |
| spring-boot-starter-amqp         | With default as RabbitMQ, this starter project provides message passing with AMQP.                                                                                                                    |
| spring-boot-starter-security     | This starter project enables auto-configuration for Spring Security.                                                                                                                                  |
| spring-boot-starter-batch        | Provides support for developing batch applications using Spring Batch.                                                                                                                                |
| spring-boot-starter-cache        | Basic support for caching using Spring Framework.                                                                                                                                                     |
| spring-boot-starter-data-rest    | Support for exposing REST services using Spring Data REST.                                                                                                                                            |

Let's use some of these goodies to code our own Spring Boot microservice.

That's it. The whole nine lines of code, not counting blank lines. It could not be more concise. The @SpringBootApplication is a kind of shortcut annotation, which is very convenient. It replaces all of the following annotations:

- @Configuration: A class marked with this annotation becomes a source of bean definitions for the application context
- @EnableAutoConfiguration: This annotation makes Spring Boot add beans based on classpath settings, other beans, and various property settings
- @EnableWebMvc: Normally you would add this one for a Spring MVC application, but Spring Boot adds it automatically when it sees spring-webmvc on the classpath. This marks the application as a web application, which in turn will activate key behaviors such as setting up a DispatcherServlet
- @ComponentScan: Tells Spring to look for other components, configurations, and services, allowing it to find the controllers

The Spring Data JPA provides a repository programming model that starts with an interface per managed domain object. Defining this interface serves two purposes. First, by extending the JPARepository interfaces, we get a bunch of generic CRUD methods into our type that allows saving our entities, deleting them, and so on. For example, the following methods are available (declared in the JPARepository interfaces we are extending):

- List<T> findAll();
- List<T> findAll(Sort sort);
- List<T> findAll(Iterable<ID> ids);
- <S extends T> List<S> save(Iterable<S> entities);
- T getOne(ID id);
- <S extends T> S save(S entity);
- <S extends T> Iterable<S> save(Iterable<S> entities);
- T findOne(ID id);
- boolean exists(ID id);
- Iterable<T> findAll();
- Iterable<T> findAll(Iterable<ID> ids);
- long count();
- void delete(ID id);
- void delete(T entity);
- void delete(Iterable<? extends T> entities);
- void deleteAll();

As you can see in the previous example, the class is annotated with the @RestController annotation. This is what makes it a controller, actually. In fact, it's a convenient annotation that is itself annotated with @Controller and @ResponseBody annotations. @Controller indicates that an annotated class is a controller (a web controller), also allowing for implementation classes to be autodetected through Spring's classpath scanning. Every method in a controller that should respond to a call to a specific URI is mapped with the @RequestMapping annotation. @RequestMapping takes parameters, the most important ones are:

- value : It will specify the URI path
- method : Specifyies the HTTP method to handle
- headers : The headers of the mapped request, in a format myHeader=myValue. A request will be handled by the method using the headers parameter, only if the incoming request header is found to have the given value
- consumes : Specifies the media types the mapped request can consume, such as "text/plain" or "application/json". This can be a list of media types, for example: {"text/plain", "application/json"}
- produces : Specifies the media types the mapped request can produce, such as "text/plain" or "application/json". This again can be a list of media types, for example: {"text/plain", "application/json"}

Similar to JAX-RS @PathParam and @QueryParam to specify the controller method's input parameters, now we have @PathVariable and @RequestParam in Spring. If you need to have your method parameter come in the request body (as a whole JSON object that you want to save, the same as in our saveBook() method), you will need to map the parameter using the @RequestBody annotation. As for the output, the @ResponseBody annotation can tell our controller that the method return value should be bound to the web response body.

# Running the application

Because we have defined the Spring Boot plugin in our pom.xml build file, we can now start the application using Maven. All you need to have is Maven present on the system path, but you probably have this already as a Java developer. To run the application, execute the following from the command shell (terminal on MacOS or cmd.exe on Windows):

```markup
$ mvn spring-boot:run 
```

# Spring Initializr

Spring Initializr is a web-based tool available at [https://start.spring.io](https://start.spring.io/). It's a quick start generator for Spring projects. Spring Initializr can be used as follows:

- From the web browser at [https://start.spring.io](https://start.spring.io/)
- In your IDE (IntelliJ IDEA Ultimate or NetBeans, using plugins)
- From the command line with the Spring Boot CLI or simply with cURL or HTTPie

Using the web application is very convenient; all you need to do is provide details about your application Maven archetype, such as group, artifact name, description, and so on:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/81cb5957-6efb-4723-8006-644326d98692.png)

In the Dependencies section, you can enter the keywords of the features you would like to have included, such as JPA, web, and so on. You can also switch the UI to an advanced view, to have all the features listed and ready to be selected:

![](https://static.packt-cdn.com/products/9781786468390/graphics/assets/6a8f5b05-30aa-4aec-92d3-64b668ef7c27.png)

As the output, Spring Initializr will create a ZIP archive with the base Maven project you want to start with. The project created by Spring Initializr is a Maven project and follows the standard Maven directory layout. This really saves a lot of time when creating new Spring projects. You no longer need to search for specific Maven archetypes and look for their versions. Initializr will generate the pom.xml for you, automatically. The presence of the dependencies in the pom.xml is important because Spring Boot will make decisions on what to create automatically when certain things are found on the classpath. For example, if the dependency for the H2 database is present and exists on the classpath when the application is run, Spring Boot will automatically create a data connection and an embedded H2 database.

https://subscription.packtpub.com/book/application-development/9781788992992/3/ch03lvl1sec22/spring-projects

# Maven app - Creating an empty application

https://subscription.packtpub.com/book/application-development/9781788390415/1/ch01lvl1sec15/creating-an-empty-application

---

We will use Maven to generate an empty application with the structure required for Java-based web applications. If you do not have Maven installed, please follow the instructions here ([Maven &#x2013; Installing Apache Maven](https://maven.apache.org/install.html)) to install Maven. Once installed, run the following command to create an empty application:

```markup
mvn archetype:generate -DgroupId=com.nilangpatel.worldgdp -DartifactId=worldgdp -Dversion=0.0.1-SNAPSHOT -DarchetypeArtifactId=maven-archetype-webapp
```

- You will see `index.jsp` added by default while creating the default project structure. You must delete it as, in this application, we will use Thymeleaf—another template engine to develop the landing page.



---



## spring-boot-devtools

```xml
        <!-- ADD SUPPORT FOR AUTOMATIC RELOADING -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>

		<!-- ADD SUPPORT FOR SPRING BOOT ACTUATOR -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
```
