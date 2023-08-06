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

## Check maven dependency

```bash
mvn dependency:tree
```

## spring-boot-devtools

```xmlag-0-1gnp6gouoag-1-1gnp6gouo
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

## JUnit 5 Dependencies

In order to kick start Junit 5 in Maven, following three dependencies/plugins
are required -

1. junit-jupiter-api - This dependency is required because it defines the API
   that we need to write Junit 5 tests.

2. junit-jupiter-engine - This dependency is required because it is the
   implementation of the junit-platform-engine API for JUnit 5. It is responsible
   for the discovery and execution of Junit 5 tests.

3. maven-surefire-plugin - This plugin is required because it is needed for
   tests to be executed during the maven build.

It also requires Java 8 or higher versions.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSche
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.hubberspot</groupId>
  <artifactId>junit5-maven-starter</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <properties>
                <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
                <maven.compiler.source>1.8</maven.compiler.source>
                <maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
                <junit.jupiter.version>5.3.2</junit.jupiter.version>
        </properties>
        <dependencies>
                <dependency>
                        <groupId>org.junit.jupiter</groupId>
                        <artifactId>junit-jupiter-api</artifactId>
                        <version>${junit.jupiter.version}</version>
                        <scope>test</scope>
                </dependency>
                <dependency>
                        <groupId>org.junit.jupiter</groupId>
                        <artifactId>junit-jupiter-engine</artifactId>
                        <version>${junit.jupiter.version}</version>
                        <scope>test</scope>
                </dependency>
        </dependencies>
        <build>
                <plugins>
                        <!-- JUnit 5 requires Surefire version 2.22.1 or higher -->
                        <plugin>
                                <artifactId>maven-surefire-plugin</artifactId>
                                <version>2.22.1</version>
                        </plugin>
                </plugins>
        </build>
</project>
```

Note: In junit 5.4, you only need to add `junit-jupiter` aggregator artifact is enough

[Mockito and JUnit 5 - Using ExtendWith | Baeldung](https://www.baeldung.com/mockito-junit-5-extension)

### @ParameterizedTest with junit-jupiter-params

Maven - In order to use parameterized tests, you need to add a dependency for

`junit-jupiter-params` to maven project. Just add below dependency to
pom.xml which we created in our earlier lesson (Junit 5 Integration with
Maven) - https://www.educative.io/collection/page/4753235730497536/569341723751219
2/5071437253574656.

```xml
<dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-params</artifactId>
        <version>${junit.jupiter.version}</version>
        <scope>test</scope>
</dependency>
```

### [Difference between junit-jupiter-api and junit-jupiter-engine](https://stackoverflow.com/questions/48448331/difference-between-junit-jupiter-api-and-junit-jupiter-engine)

#### `junit-jupiter` aggregator artifact

[JUnit 5.4 provides](https://hub.packtpub.com/junit-5-4-released-with-an-aggregate-artifact-for-reducing-your-maven-and-gradle-files/) much simpler Maven configuration if your intent is to write JUnit 5 tests. Simply specify the aggregate artifact named [`junit-jupiter`](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter).

```xml
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
```

As an aggregate, this artifact in turn pulls the following three artifacts automatically, for your convenience:

- [`junit-jupiter-api`](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api) (a compile dependency)
- [`junit-jupiter-params`](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-params) (a compile dependency)
- [`junit-jupiter-engine`](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine) (a runtime dependency)
  - `junit-jupiter-api` is included as a sub-dependency in `junit-jupiter-engine` Maven repository. So you'll only really need to add `junit-jupiter-engine` to get both

#### Legacy tests Junit 3, 4 with `junit-vintage-engine`

If your project has JUnit 3 or 4 tests that you want to continue to run, add another dependency for the *JUnit Vintage Engine*, [`junit-vintage-engine`](https://mvnrepository.com/artifact/org.junit.vintage/junit-vintage-engine). See [tutorial by IBM](https://developer.ibm.com/tutorials/j-introducing-junit5-part2-vintage-jupiter-extension-model/).

```xml
<!-- https://mvnrepository.com/artifact/org.junit.vintage/junit-vintage-engine -->
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
```

Conclusion:

- You need both `junit-jupiter-api` and `junit-jupiter-engine` to write and run JUnit5 tests
- You only need `junit-vintage-engine` if (a) you are running with JUnit5 **and** (b) your test cases use JUnit4 constructs/annotations/rules etc

## Junit 5 and mockito

### The Difference Between mockito-core and mockito-all

[The Difference Between mockito-core and mockito-all | Baeldung](https://www.baeldung.com/mockito-core-vs-mockito-all)

**[The *mockito-core* artifact](https://search.maven.org/artifact/org.mockito/mockito-core) is Mockito's main artifact.** Specifically, it contains both the API and the implementation of the library. => Use it

Of course, *mockito-core* has some dependencies like *hamcrest* and *objenesis* that Maven downloads separately, but *mockito-all* is **an out-dated dependency** that bundles Mockito as well as its required dependencies. => don't use

The latest GA version of [*mockito-all*](https://search.maven.org/artifact/org.mockito/mockito-all) is a 1.x version released in 2014. **Newer versions of Mockito don't release *mockito-all* anymore**.

### Configuration

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>3.3.3</version>
</dependency>
```

=> spring-boot-starter-test also include mockito-junit-jupiter:jar:3.3.3 and junit-jupiter:jar:5.6.2

check dependency of spring boot and mockito

```bash
[INFO] +- org.springframework.boot:spring-boot-starter-test:jar:2.3.0.RELEASE:test
[INFO] |  +- org.springframework.boot:spring-boot-test:jar:2.3.0.RELEASE:test
[INFO] |  +- org.springframework.boot:spring-boot-test-autoconfigure:jar:2.3.0.RELEASE:test
[INFO] |  +- com.jayway.jsonpath:json-path:jar:2.4.0:test
[INFO] |  |  \- net.minidev:json-smart:jar:2.3:compile
[INFO] |  |     \- net.minidev:accessors-smart:jar:1.2:compile
[INFO] |  +- jakarta.xml.bind:jakarta.xml.bind-api:jar:2.3.3:compile
[INFO] |  |  \- jakarta.activation:jakarta.activation-api:jar:1.2.2:compile
[INFO] |  +- org.assertj:assertj-core:jar:3.16.1:test
[INFO] |  +- org.hamcrest:hamcrest:jar:2.2:test
[INFO] |  +- org.junit.jupiter:junit-jupiter:jar:5.6.2:test
[INFO] |  |  +- org.junit.jupiter:junit-jupiter-api:jar:5.6.2:test
[INFO] |  |  |  +- org.apiguardian:apiguardian-api:jar:1.1.0:test
[INFO] |  |  |  +- org.opentest4j:opentest4j:jar:1.2.0:test
[INFO] |  |  |  \- org.junit.platform:junit-platform-commons:jar:1.6.2:test
[INFO] |  |  +- org.junit.jupiter:junit-jupiter-params:jar:5.6.2:test
[INFO] |  |  \- org.junit.jupiter:junit-jupiter-engine:jar:5.6.2:test
[INFO] |  |     \- org.junit.platform:junit-platform-engine:jar:1.6.2:test
[INFO] |  +- org.mockito:mockito-junit-jupiter:jar:3.3.3:test
[INFO] |  +- org.skyscreamer:jsonassert:jar:1.5.0:test
[INFO] |  |  \- com.vaadin.external.google:android-json:jar:0.0.20131108.vaadin1:test
[INFO] |  +- org.springframework:spring-test:jar:5.2.6.RELEASE:test
[INFO] |  \- org.xmlunit:xmlunit-core:jar:2.7.0:test
[INFO] +- org.mockito:mockito-core:jar:3.3.3:test
[INFO] |  +- net.bytebuddy:byte-buddy:jar:1.10.10:compile
[INFO] |  +- net.bytebuddy:byte-buddy-agent:jar:1.10.10:test
[INFO] |  \- org.objenesis:objenesis:jar:2.6:test
```

**Error**: [[Solved] IllegalStateException: Could not initialize plugin MockMaker](https://howtodoinjava.com/mockito/plugin-mockmaker-error/)

=> Mockito core depends on a library called **byte-buddy,** and this problem mostly occurs when mockito doesn’t find a matching byte-buddy jar version

It always get version 1.10.10

```xml
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>4.11.0</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/net.bytebuddy/byte-buddy -->
        <dependency>
            <groupId>net.bytebuddy</groupId>
            <artifactId>byte-buddy</artifactId>
            <version>1.12.19</version>
        </dependency>
```

### Building the Test Class with @Mock

[Mockito and JUnit 5 - Using ExtendWith | Baeldung](https://www.baeldung.com/mockito-junit-5-extension)

Let’s build our test class and attach the Mockito extension to it:

```java
@ExtendWith(MockitoExtension.class)
class UserServiceUnitTest {

    UserService userService;

    // ...
}
```

If you use the `@Mock` annotation, you must trigger the initialization of the annotated fields. The `MockitoExtension` does this by calling the static method `MockitoAnnotations.initMocks(this)`.

We can use the *@Mock* annotation to inject a mock for an instance variable that we can use anywhere in the test class:

```java
@Mock UserRepository userRepository;
```

https://www.vogella.com/tutorials/Mockito/article.html

### Difference Between @Mock and @InjectMocks in Mockito

[Difference Between @Mock and @InjectMocks in Mockito - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-mock-and-injectmocks-in-mockito/)

> **@Mock creates a mock, and @InjectMocks creates an instance of the class and injects the mocks that are created with the @Mock annotations into this instance.**

[How to Write Test Cases in Java Application using Mockito and Junit? - GeeksforGeeks](https://www.geeksforgeeks.org/how-to-write-test-cases-in-java-application-using-mockito-and-junit/?ref=gcse)

## Junit 5 and spring-boot

=> spring-boot-starter-test also include mockito-junit-jupiter:jar:3.3.3 and junit-jupiter:jar:5.6.2

### How to properly ignore Junit 4 in Gradle and Maven

[How to properly ignore Junit 4 in Gradle and Maven - DEV Community ](https://dev.to/art_ptushkin/how-to-properly-ignore-junit-4-in-gradle-and-maven-3o82)

#### Maven

[Spring Boot: `'junit-vintage' failed to discover tests` When Using Only JUnit5 Tests &#183; Jamie Tanna | Software Engineer](https://www.jvt.me/posts/2020/06/30/spring-boot-junit-vintage-failed-discover/)

It turns out, as of 2.3.1.RELEASE, I also need to now *either* remove my `<exclusions>`:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <version>${spring-boot.version}</version>
</dependency>
```

Or exclude both JUnit 4 and JUnit's "Vintage" engine [which allows running both JUnit4 and JUnit5 code side-by-side](https://www.jvt.me/posts/2019/12/23/junit4-junit5-gotcha-together-maven/):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    <exclusions>
      <!-- exclude junit 4 -->
      <exclusion>
        <groupId>org.junit.vintage</groupId>
        <artifactId>junit-vintage-engine</artifactId>
      </exclusion>
      <exclusion>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
      </exclusion>
    </exclusions>
  </dependency>
```

https://medium.com/@shaunthomas999/junit-5-jupiter-dependency-management-with-maven-in-spring-boot-application-a5e7186b30eb

[How to enable JUnit 5 in new Spring Boot project - DEV Community ](https://dev.to/martinbelev/how-to-enable-junit-5-in-new-spring-boot-project-29a8)

#### Gradle

Having [configurations in Gradle](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.Configuration.html) it is easy to exclude a dependency in one particular place:

```js
/* for all the configurations */
configurations {
  all {
    /* only junit 5 should be used */
    exclude group: 'junit', module: 'junit'
    exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
  }
}
```

```js
/* for a particular test module */
configurations {
  testCompile {
  /* only junit 5 should be used */
    exclude group: 'junit', module: 'junit'
    exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
  }
  testRuntime {
    /* only junit 5 should be used */
    exclude group: 'junit', module: 'junit'
    exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
  }
}
```

### Full pom.xml file

[Spring Boot + JUnit 5 + Mockito - Mkyong.com](https://mkyong.com/spring-boot/spring-boot-junit-5-mockito/)

[Junit 5 with Spring boot 2 - HowToDoInJava](https://howtodoinjava.com/spring-boot2/testing/junit5-with-spring-boot2/)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mkyong.spring</groupId>
    <artifactId>testing-junit5-mockito</artifactId>
    <version>1.0</version>

    <properties>
        <java.version>1.8</java.version>
        <junit-jupiter.version>5.3.2</junit-jupiter.version>
        <mockito.version>2.24.0</mockito.version>
    </properties>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.0.RELEASE</version>
    </parent>

    <dependencies>

        <!-- mvc -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- exclude junit 4 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>junit</groupId>
                    <artifactId>junit</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- junit 5: Only mockito-core is enough, spring-boot-starter-test contains it -->
<!--
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.9.1</version>
            <scope>test</scope>
        </dependency>
-->

        <!-- mockito -->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>3.3.3</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springdoc</groupId>
            <artifactId>springdoc-openapi-webmvc-core</artifactId>
            <version>${springdoc.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
           <!-- Spring boot already includes a default configuration for running tests.
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
            </plugin>
           -->

        </plugins>
    </build>

</project>
```

junit-jupiter-engine alse includes junit-jupiter-api OR you can use junit-jupiter

In a typical Spring Boot project, you don't need to explicitly configure the `maven-surefire-plugin` for running tests. Spring Boot already includes a default configuration for running tests.

### @SpringBootTest

With Junit 5, we do not need `@RunWith(SpringRunner.class)` anymore. Spring tests are executed with `@ExtendWith(SpringExtension.class)` and `@SpringBootTest` and the other `@…Test` annotations are already annotated with it.

Add [application.properties](http://application.properties/) in resources folder to test with Spring

Every new Spring Boot project comes with an initial test inside src/test/resources that uses this annotation:

```java
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class DemoApplicationTest {

  @Test
  void contextLoads() {
  }

}
```
