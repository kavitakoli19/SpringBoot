# DemoApplication.java
```
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```

# TodoController.java
```
package com.example.demo.web.controller;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.SessionAttributes;

import com.example.demo.web.model.Todo;
import com.example.demo.web.service.TodoService;

@Controller
@SessionAttributes("name")
public class TodoController {
	
	@Autowired
	TodoService service;
	
	@RequestMapping(value="/login", method = RequestMethod.GET)
	public String login(ModelMap model){
		
		return "login";
	}
	
	

	@RequestMapping(value="/login", method = RequestMethod.POST)
	public String loginPost(ModelMap model,@RequestParam String name,@RequestParam String password){
		
		model.put("name", name);
		
		if(password.equals("Anonymous"))
			return "welcome";
		else
			return "error";
	}
	
	@RequestMapping(value="/list-todos",method = RequestMethod.GET)
	public String showTodos(ModelMap model) {
		
		
		String name = (String) model.get("name");
		
		model.put("todos", service.retrieveTodos(name));
		
		return "list-todos";		
		
	}
	
	@RequestMapping(value="/addtodo",method = RequestMethod.POST)
	public String addTodoPost(ModelMap model,@Valid Todo todo , BindingResult result) {
		if(result.hasErrors()) {
			return "addtodo";
		}
		
		service.addTodo((String)model.get("name"), todo.getDesc(), new java.util.Date(), false);
		
		return "redirect:/list-todos";		
		
	}
	
	@RequestMapping(value="/addtodo",method = RequestMethod.GET)
	public String addToDoGet(ModelMap model) {
		
		model.addAttribute("todo", new Todo(0, (String)model.get("name"), "test", new java.util.Date(),false));
		
		return "addtodo";		
		
	}
	
	@RequestMapping(value="/deleteTodo",method= RequestMethod.GET)
	public String deleteTodo(@RequestParam int id) {
		
		service.deleteTodo(id);
		
		return "redirect:/list-todos"; 
	}	
}

```

# Todo.java
```
package com.example.demo.web.model;

import java.util.Date;

import javax.validation.constraints.Size;

public class Todo {
    private int id;
    private String user;
    
    @Size(min=10,message="Please enter at least 10 chars....")
    private String desc;
    
    
    private Date targetDate;
    private boolean isDone;
    
    
   public Todo() {
		super();
	}
	
  
	public Todo(int id, String user, String desc, Date targetDate,
            boolean isDone) {
        super();
        this.id = id;
        this.user = user;
        this.desc = desc;
        this.targetDate = targetDate;
        this.isDone = isDone;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public Date getTargetDate() {
        return targetDate;
    }

    public void setTargetDate(Date targetDate) {
        this.targetDate = targetDate;
    }

    public boolean isDone() {
        return isDone;
    }

    public void setDone(boolean isDone) {
        this.isDone = isDone;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + id;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        Todo other = (Todo) obj;
        if (id != other.id) {
            return false;
        }
        return true;
    }

    @Override
    public String toString() {
        return String.format(
                "Todo [id=%s, user=%s, desc=%s, targetDate=%s, isDone=%s]", id,
                user, desc, targetDate, isDone);
    }

}
```

# TodoService.java
```
package com.example.demo.web.service;

	import java.util.ArrayList;
	import java.util.Date;
	import java.util.Iterator;
	import java.util.List;

	import org.springframework.stereotype.Service;

	import com.example.demo.web.model.Todo;

	@Service
	public class TodoService {
	    private static List<Todo> todos = new ArrayList<Todo>();
	    private static int todoCount = 3;

	    static {
	        todos.add(new Todo(1, "Anonymous", "Learn Spring MVC", new Date(),
	                false));
	        todos.add(new Todo(2, "Anonymous", "Learn Struts", new Date(), false));
	        todos.add(new Todo(3, "Anonymous", "Learn Hibernate", new Date(),
	                false));
	    }

	    public List<Todo> retrieveTodos(String user) {
	        List<Todo> filteredTodos = new ArrayList<Todo>();
	        for (Todo todo : todos) {
	            if (todo.getUser().equals(user)) {
	                filteredTodos.add(todo);
	            }
	        }
	        return filteredTodos;
	    }

	    public void addTodo(String name, String desc, Date targetDate,
	            boolean isDone) {
	        todos.add(new Todo(++todoCount, name, desc, targetDate, isDone));
	    }

	    public void deleteTodo(int id) {
	        Iterator<Todo> iterator = todos.iterator();
	        while (iterator.hasNext()) {
	            Todo todo = iterator.next();
	            if (todo.getId() == id) {
	                iterator.remove();
	            }
	        }
	    }
	}

```

# src\main\resources\application.properties
```
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
logging.level.spring.framework.web=debug
```

# src\main\webapp\WEB-INF\jsp\addtodo.jsp
```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<%@taglib uri="http://www.springframework.org/tags/form" prefix="form"%>

<!DOCTYPE html>
<html>
<head>

<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
	    		rel="stylesheet">


<meta charset="ISO-8859-1">
<title>Add To DO page</title>
</head>
<body>
	    		
 <%-- <div>
	<form method="post" class="form-group">
		Description : <input name = "desc" type ="text" class="form-control"/> 
		<input type = "submit" />
	</form>
</div>  --%>


<div class="form-group">
	<form:form method="post" modelAttribute="todo" >
		<form:label path="desc" >Description</form:label>
		<form:input path = "desc" type ="text" class="form-control" required = "required" value="sample value" /> 
		<form:button type = "submit" class = "btn btn-success"> Submit</form:button>
		<form:errors path="desc"></form:errors>
	</form:form>
</div>


<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
</body>
</html>
```

# src\main\webapp\WEB-INF\jsp\error.jsp
```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Error Page</title>
</head>
<body>
Something went wrong!
 </body>
</html>
```

# src\main\webapp\WEB-INF\jsp\list-todos.jsp
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>


<html>
<head>

<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
	    		rel="stylesheet">

<title>Todo's for ${name}</title>
</head>
<body>

	

	<table class="table table-striped">
		<caption><H1>Your todo's are</H1></caption>
		<thead>
			<tr>
				<th>Description</th>
				<th>Target Date</th>
				<th>Is it done?</th>
				<th>Delete</th>
			</tr>
		</thead>

		<tbody>
			<c:forEach items="${todos}" var="todo">
				<tr>
					<td>${todo.desc}</td>
					<td>${todo.targetDate}</td> 
					<td>${todo.isDone()}</td>
					<td><a href = "/deleteTodo?id=${todo.getId()}" class="btn btn-warning"> Delete </a></td>
					
				</tr>
			</c:forEach>
		</tbody>
	</table>


	<a href="/addtodo"> Add a to do </a>

<BR/><BR/>
<H3>
Here please note that boolean variable is called using getter method.
This is an exception which we found,accessing boolean variable directly in jsp could not identify the variable.
We have used getter for only boolean and rest two values are accessed by varibales only.
Also al vaues are private stil it worked. </H3>


<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>

</body>
</html>
```

# src\main\webapp\WEB-INF\jsp\login.jsp
```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Welcome Page</title>
</head>
<body>
<form method="post">
	Name : <input type ="text" name = "name"/>
	Password : <input type = "password" name ="password"/>
	<input type = "submit" />
	</form>
</body>
</html>
```


# src\main\webapp\WEB-INF\jsp\welcome.jsp
```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Welcome Page</title>
</head>
<body>
welcome ${name} <a href ="/list-todos"> Click here</a> to manage to dos.
</body>
</html>
```

# pom.xml
``` 
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.6.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>
	<description>Demo project for Spring Boot To Do List</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<version>8.5.50</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>


		<dependency>
			<groupId>javax.servlet.jsp.jstl</groupId>
			<artifactId>javax.servlet.jsp.jstl-api</artifactId>
			<version>1.2.1</version>
		</dependency>



		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>


		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap</artifactId>
			<version>3.3.6</version>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>1.9.1</version>
		</dependency>


	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```
