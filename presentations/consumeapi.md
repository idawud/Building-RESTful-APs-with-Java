# Consuming APIs

## Notes
* [Spring boot RestTemplate](https://javadeveloperzone.com/spring-boot/spring-boot-resttemplate-example/)

* [Consuming a RESTful Web Service with jQuery](https://spring.io/guides/gs/consuming-rest-jquery/)

* [GET and POST requests using Python](https://www.geeksforgeeks.org/get-post-requests-using-python/)

* [Cross-Origin Resource Sharing (CORS)]( https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

* [How the Access-Control-Allow-Origin Header Works](https://medium.com/@dtkatz/3-ways-to-fix-the-cors-error-and-how-access-control-allow-origin-works-d97d55946d9)

### 1. Introduction
Consuming an API: means creating a client which can send requests to the API that you build.

We can use `GET` on our web API `https://javarestfulapi.herokuapp.com/todos` to fetch all our todos to be used in our application. It can be an application in any other programming language.

-----------------------------------------------------------------------------------------------
 
## Lab 5 Consume APIs
In this Lab we'll try to consume the web api we've created using applications in 3 different languages.

### Java
There are many ways to do this in java, but I'll be working with RestTemplateBuilder class provided by spring boot web.

``` Java 
import com.restful.RestfulApi.model.Todo;
import org.springframework.boot.web.client.RestTemplateBuilder;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class RestfulApiApplication {
	private static RestTemplateBuilder builder = new RestTemplateBuilder();

	public static void main(String[] args) {
		Optional<List<Todo>> fectchedTodos = getTodosFromAPI();
		if( fectchedTodos.isPresent()){
			fectchedTodos.get().forEach(
					todo -> System.out.println( "ID -> " + todo.getId() + "\t\tMessage -> " + todo.getMessage())
			);
		}else {
			System.out.println("Empty Todo");
		}
	}

	public static Optional<List<Todo>> getTodosFromAPI(){
		Todo[] todos = builder.build().getForObject("https://javarestfulapi.herokuapp.com/todos", Todo[].class);
		if ( todos == null){
			return Optional.empty();
		}
		return Optional.of(Arrays.asList(todos));
	}
}

```
Running this, it will fetch all the todos and print them out.<br>
![Consume with Java]( /presentations/images/java.png)


### Python
Same can be done using the requests librarry which is provided by default using python3, so there is no need for an pip install.
``` Python
import requests

res = requests.get('https://javarestfulapi.herokuapp.com/todos')

print(res.json())
```
Running this piece of code also yield the same result.
![Consume with Python]( /presentations/images/pythonconsume.png)

### JavaScript
``` JavaScript
fetch('https://javarestfulapi.herokuapp.com/todos')
    .then((response) => { return response.json(); })
    .then((data) => { console.log(data); });
```

Now Running this piece of code also yield the same result.
![Consume with JavaScript]( /presentations/images/javaScript.png)