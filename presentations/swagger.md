# APIs Documentation with Swagger
[Swagger UI](https://swagger.io/tools/swagger-ui/)
[Advance Swagger](https://springfox.github.io/springfox/docs/current/)

## 1. Introduction
Swagger UI allows anyone — be it your development team or your end consumers — to visualize and interact with the API’s resources without having any of the implementation logic in place. It’s automatically generated from your OpenAPI (formerly known as Swagger) Specification, with the visual documentation making it easy for back end implementation and client side consumption. <br/>
The Springfox suite of java libraries are all about automating the generation of machine and human readable specifications for JSON APIs written using the spring family of projects. Springfox works by examining an application, once, at runtime to infer API semantics based on spring configurations, class structure and various compile time java Annotations.
<br/>
[Good Read on Swagger Documentation](https://www.baeldung.com/swagger-2-documentation-for-spring-rest-api)
-----------------------------------------------------------------------------------------------
 
## Lab 2 Swagger Documentation
In it's okay to test your APIs with Postman. But on large projects where there are multiple endpoints and lots of people involve it becomes tedious because you'll have to remember all the endpoints and all the data models. That's where API documentations help.

### Step 1 - Adding Swagger Dependency
The `build.gradle` in the root of the project contains all the external depencies for the projects such as the spring boot framework itself. So we'll be adding swagger library as such.

``` Dependency
      compile("io.springfox:springfox-swagger2:2.9.2")
	  compile("io.springfox:springfox-swagger-ui:2.9.2")
```
![Swagger Dependency]( /presentations/images/swagger-dep.png)

### Step 2 - Adding Swagger Annotations
1. Add the `@EnableSwagger2` annotation to your main Class, to eable swagger on thhis projects.
![Swagger Enable]( /presentations/images/swagger-enable.png)
2. Add the `@Api` annotation to the controller and `@ApiOperation(&lg; description &gt;)` to the endpoint functions with the desription of what the endpoint does.
![Swagger Endpoint Trace]( /presentations/images/swagger-api.png)

### Step 3 -  Swagger Page
Start or REstart your server and navigate to [localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
![Swagger Page]( /presentations/images/swagger-page.png)

Click on the `todo-controller` and you'll see all the endpoints under this controller. From here we can do all the stuffs we did with POSTMAN.
![Swagger Todo Endpoints]( /presentations/images/swagger-todo-endpoints.png)
