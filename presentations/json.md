# Data Serialization & Deserialization JSON 


## Notes
* [Jackson ObjectMapper ]( http://tutorials.jenkov.com/java-json/jackson-objectmapper.html)  
* [Intro to the Jackson ObjectMapper](https://www.baeldung.com/jackson-object-mapper-tutorial )

### 1. Introduction

This write-up focuses on understanding the Jackson ObjectMapper class – and how to serialize Java objects into JSON and deserialize JSON string into Java objects. To understand more about the Jackson library in general which spring-boot uses by default.

### 2. Serialization Java Object to JSON
Let's see a first example of serializing a Java Object into JSON using the writeValue method of ObjectMapper class:
```
ObjectMapper objectMapper = new ObjectMapper();
Car car = new Car("yellow", "renault");
objectMapper.writeValue(new File("target/car.json"), car);
```
The output of the above in the file will be:
```
{"color":"yellow","type":"renault"}
```
The methods writeValueAsString and writeValueAsBytes of ObjectMapper class generates a JSON from a Java object and returns the generated JSON as a string or as a byte array:
```
String carAsString = objectMapper.writeValueAsString(car);
```


### 3. Deserialization JSON to Java Object
Below is a simple example of converting a JSON String to a Java object using the ObjectMapper class:
```
String json = "{ \"color\" : \"Black\", \"type\" : \"BMW\" }";
Car car = objectMapper.readValue(json, Car.class);
```
The readValue() function also accepts other forms of input like a file containing JSON string:
```
Car car = objectMapper.readValue(new File("src/test/resources/json_car.json"), Car.class);
```
or a URL:
```
Car car = objectMapper.readValue(new URL("file:src/test/resources/json_car.json"), Car.class);
```

#### 3.1 JSON to Jackson JsonNode
Alternatively, a JSON can be parsed into a JsonNode object and used to retrieve data from a specific node:
```
String json = "{ \"color\" : \"Black\", \"type\" : \"FIAT\" }";
JsonNode jsonNode = objectMapper.readTree(json);
String color = jsonNode.get("color").asText();
// Output: color -> Black
```

#### 3.2 Creating a Java List from a JSON Array String
We can parse a JSON in the form of an array into a Java object list using a TypeReference:
```
String jsonCarArray = "[{ \"color\" : \"Black\", \"type\" : \"BMW\" }, { \"color\" : \"Red\", \"type\" : \"FIAT\" }]";
List<Car> listCar = objectMapper.readValue(jsonCarArray, new TypeReference<List<Car>>(){});
```

#### 3.3 Creating Java Map from JSON String
Similarly, we can parse A JSON into a Java Map:
```
String json = "{ \"color\" : \"Black\", \"type\" : \"BMW\" }";
Map<String, Object> map = objectMapper.readValue(json, new TypeReference<Map<String,Object>>(){});
```
### 4. Advanced Features
One of the greatest strength of the Jackson library is the highly customizable serialization and deserialization process.

In this section, we'll go through some advanced features where the input or the output JSON response can be different from the object which generates or consumes the response.

#### 4.1 Configuring Serialization or Deserialization Feature
While converting JSON objects to Java classes, in case the JSON string has some new fields, then the default process will result in an exception:
```
String jsonString = "{ \"color\" : \"Black\", \"type\" : \"Fiat\", \"year\" : \"1970\" }";
```
The JSON string in the above example in the default parsing process to the Java object for the Class Car will result in the UnrecognizedPropertyException exception.

Through the configure method we can extend the default process to ignore the new fields:
```
objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
Car car = objectMapper.readValue(jsonString, Car.class);
 
JsonNode jsonNodeRoot = objectMapper.readTree(jsonString);
JsonNode jsonNodeYear = jsonNodeRoot.get("year");
String year = jsonNodeYear.asText();
```
Yet another option is based on the __FAIL_ON_NULL_FOR_PRIMITIVES__ which defines if the null values for primitive values are allowed:
```
objectMapper.configure(DeserializationFeature.FAIL_ON_NULL_FOR_PRIMITIVES, false);
```
Similarly, __FAIL_ON_NUMBERS_FOR_ENUM__ controls if enum values are allowed to be serialized/deserialized as numbers:
```
objectMapper.configure(DeserializationFeature.FAIL_ON_NUMBERS_FOR_ENUMS, false);
```
You can find the comprehensive list of serialization and deserialization features on the [official site](https://github.com/FasterXML/jackson-databind/wiki/Serialization-Features).