# Creating API with Spring Boot 

## Notes
[Official Spring Boot](https://spring.io/guides/gs/spring-boot/) Documentation

### 1. Introducing Spring Boot
The Spring team decided they wanted to provide developers with some utilities which relatively automate the configuration procedures and speeds up the process of building and deploying Spring applications, so they invented Spring Boot.

*Spring Boot* is a utility project which aims to make it easy to build Spring-based, production-ready applications and services with minimum fuss. It provides the shortest way to get a Spring web application up and running with the smallest line of code/configuration out-of-the-box.

### 2. Spring Boot Features
There are a bunch of features specific to Spring Boot, but three of my favorites are dependency management, auto-configuration, and embedded servlet containers.

#### 2.1. Easy Dependency Management
In order to speed up the dependency management process, Spring Boot implicitly packages the required compatible third-party dependencies for each type of Spring application and exposes them to the developer using starters.

Starters are a set of convenient dependency descriptors that you can include in your application. You get a one-stop-shop for all the Spring and related technology that you need, without having to hunt through sample code and copy paste loads of dependency descriptors.

For example, if you want to get started using Spring and JPA for database access, just include the spring-boot-starter-data-jpa dependency in your project, and you are good to go (no need to hunt for compatible database drivers and Hibernate libraries).

Also, if you want to create a Spring web application, just add spring-boot-starter-web dependency, and, by default, this will pull all the commonly used libraries for developing Spring MVC applications such as spring-webmvc, jackson-json, validation-api, and Tomcat.

In other words, Spring Boot gathers all the common dependencies and defines them in one place and allows the developer to use them instead of reinventing the wheel each time they create a new application.

Therefore, pom.xml becomes much smaller than the one used with traditional Spring applications.

Check the docs to get familiar with all Spring Boot starters.

#### 2.2. Auto Configuration
The second awesome feature of Spring Boot is the auto-configuration.

After you select the appropriate starter, Spring Boot attempts to automatically configure your Spring application based on the jar dependencies that you have added.

For example, if you add spring-boot-starter-web, Spring Boot automatically configures the commonly registered beans like DispatcherServlet, ResourceHandlers, MessageSource.

Also, if you’re using spring-boot-starter-jdbc, Spring Boot automatically registers the DataSource, EntityManagerFactory, and TransactionManager beans and reads the connection details from the application.properties file.

In case you don't intend to use a database and you don't provide any manual connection details, Spring Boot will auto-configure an in-memory database without any further configuration on your part whenever it finds an H2 or HSQL library on the build path.

This is totally configurable and can be overridden anytime by custom configuration.

#### 2.3 Embedded Servlet Container Support
Each Spring Boot web application includes an embedded web server by default, check this for the list of embedded servlet containers supported out of the box.

Developers don’t need to worry about setting up a servlet container and deploying the application on it. The application can be run by itself as a runnable jar file using its embedded server.

If you need to use a separate HTTP server, you just need to exclude the default dependencies, Spring Boot provides separate starters for HTTP servers to help make this process as easy as possible.

Creating standalone web applications with embedded servers is not only convenient for development but also a legitimate solution for enterprise-level applications, and is increasingly useful in the microservices world. Being able to wrap an entire service (for example, user authentication) in a standalone and fully-deployable artifact that exposes an API makes distribution and deployment much quicker and easier to manage.


<br></br><br></br>
-----------------------------------------------------------------------------------------------
<br></br><br></br>

## Lab 1 Spring Boot

We are going to create a project using Spring Boot. Take a moment to think about how you have created projects in the past....

* Created from a random template on the internet?
* Copy and pasted from StackOverflow until something worked?
* Found a project within an organisation and used the same thing (just renaming a few things)

Our workshop starts with a empty folder. We are going to use [SpringInitalizr](https://start.spring.io) to create our project and get started.

### Step 1 - Generating the Project
If you've already completed the [pre-requisites](../prerequisites/README.md) you can skip forward to Step 3.

* Visit [https://start.spring.io](https://start.spring.io) to create a new project
* Choose a gradle project for the lab
* We will use Java
* Select a **2.2.6** version of Spring Boot
* Create some project metadata that makes sense
* Add in the required dependencies, to get us started we will add
   * The Spring Web dependency, which is required for tomcat to be embedded

![Spring Initializr]( /presentations/images/spring-inti.png)

Generating the project will download the dependencies and structure of a working Spring Application.

Alternatively, use this [link](https://start.spring.io/#!type=gradle-project&language=java&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.restful&artifactId=RestfulApi&name=RestfulApi&description=Demo%20project%20for%20Spring%20Boot&packageName=com.restful.RestfulApi&dependencies=web) to directly download the base project.


### Step 2 - Importing the Project

#### Intellij

Import the project into your IDE by opening the the `build.gradle` file as a project.

![Intellij Project Import](01B-sample-import.png)

On hitting OK the project will most likely download the internet (or at least all the required dependencies). 
Once this has completed your baseline project is ready. 
You can try running the tests to verify that your project builds and the context loads correctly.
 

### Step 3 - Creating our First Controller

In the project we will create a small REST controller. 
Spring boot works by scanning classes and looking for annotations it recognises.
Based on these annotations it will be opinionated and choose what it thinks the right set of configuration should look like.

Usually controllers would live in their own package, you should create the `WorkshopController.java` in your project.

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class WorkshopController {

    @RequestMapping("/hello")
    @ResponseBody
    public String helloWorld() {
        return "Hello World";
    }

}
```

We can now start our project by running `ApiworkshopApplication.java` (or the class which contains the main method). 
In a couple of seconds you should be able to visit [http://localhost:8080/hello](http://localhost:8080/hello)

![Hello World](01C-hello-world.png)

### Step 4 - Building a (slightly) more advanced API

We will create a very simple todo list application.
The application doesn't need to persist any data, though if you wanted to continue this exercise afterwards that is one extension point.

Here are the operations that we will look to implement:

* `GET /todos` Returns a list of todo items, initially the list will be empty
* `POST /todos` Creates a todo item
* `GET /todos/1` Returns the todo item with the ID 1
* `GET /todos?done=false` Returns a list of todo items, excluding those that are already done
* `DELETE /todos/1` Remove the todo item 1

#### Design considerations

* You will need a POJO to represent the task (which contains an id and a description)
* You will need to explore the `@PathVariable` annotation
* You will probably want to factor out the Tasks features from the actual controller. 
This will allow you to test independently. 

You can test your API from your IDE or install a free tool like [Tabbed Postman - REST Client](https://chrome.google.com/webstore/detail/tabbed-postman-rest-clien/coohjcphdfgbiolnekdpbcijmhambjff) 
into your browser. 
 