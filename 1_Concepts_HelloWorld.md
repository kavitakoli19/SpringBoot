# Getting started 
  - Get a spring project by using spring initializr
    https://start.spring.io/
  - Fill in the necessary details and also add dependacies as Web and Devtools.
  - Download this porject and while importing to eclipse import as "Existing Maven Project".
  - Now our project is set up to work on.
  
  
# pom.xml
  - This signifies the porject identification which we give as part of the spring initializr while creating a project.
  ```
    <groupId>com.example.spring.web.application</groupId>
	  <artifactId>v1</artifactId>
  	<version>0.0.1-SNAPSHOT</version>
  	<name>v1</name>
	  <description>Demo project for Spring Boot</description>
   ```
   - These adds necessary maven dependancies required to execute the porject,to add the annotations to project,to build and compile project everytime we make change to it. Also support for unit testing framework.
   
   ```
   <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
```

# Flow : 
- V1Application.java
  - Starting point for the spring application.

- LoginController.java
  - Compiler looks for @Controller class to map any URL hit on server.
  - URL is matched with the value given in @RequestMapping,to identify its mapping for this request. 
    - value attribute can have multiple values as well ex. @RequestMapping(value={"", "/page", "page*","view/*,**/msg"})
    - This will map to all below URLs
       - localhost:8080/home
       - localhost:8080/home/
       - localhost:8080/home/page
       - localhost:8080/home/pageabc
       - localhost:8080/home/view/
       - localhost:8080/home/view/view
  - @ResponseBody will return a string which will be shown directly as a web page content.

# src\main\resources\application.properties
  - We can change logging level for any package with syntax "logging.level.name_of_the_package = debug"
```
logging.level.org.springframework.web=DEBUG
```
  

# Annotations :

  - @SpringBootApplication :: Marks the starting point of the spring application.
  - @Controller            :: Marks class as a Controller which is a special form of a @Component and is autodetected through classpath                                 scanning.
  - @RequestMapping :: Marks methods of MVC and REST controllers as handler to HTTP requests. Can be used on a class or a method.
  - @ResponseBody :: Annotation that indicates a method return value should be bound to the web response body. Supported for annotated handler methods
 

# How to execute
 - Run the main application
 - hit URL http://localhost:8080/login
