# Creating API with Spring Boot 

## Notes

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

You can test your API from your IDE or install a free tool like [Restlet](https://chrome.google.com/webstore/detail/restlet-client-rest-api-t/aejoelaoggembcahagimdiliamlcdmfm?hl=en) 
into your browser. 
 