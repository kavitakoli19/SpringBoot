 Things to cover :: 
 
 A. Bootstrap Settings:: 
 
   a. Web jars Dependancies ::  Add to pom.xml
   
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
           
      
   b. Add to JSP pages :: 
     
    	1. Add this, in the begining of JSP File :: 
     
     		<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet">
          
        2. Add this, to end of the JSP File ::
     
		<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
	  	<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
		
		
		
B. In this example we are creating a TODOs list for a user and updating it with add and delete operations.
C. Also adding a styling to form controls using using bootstrap.
D. Using JSTL tags to iterate list of todos while displaying the content on web page.


a. Add operation  ::

	1. JSP ::
	
		. A form with Text box for description and a Submit button.
	
			<form method="post">
				Description : <input name = "desc" type ="text"/> 
				<input type = "submit" />
			</form>

		
	2. Controller :: 
	
	 	. GET Method ::
		 	. To display the addtodo page with the form controls to add the new todo item.

				@RequestMapping(value="/addtodo",method = RequestMethod.GET)
				public String addToDoGet(ModelMap model) {
					return "addtodo";		
				}
			
		
		. POST Method ::
			
			. redirect:/list-todos =>
			     . This will take the controller to the method whose RequestMapping(value = "/list-todos")
			  
		
			@RequestMapping(value="/addtodo",method = RequestMethod.POST)
			public String addTodoPost(ModelMap model,@RequestParam String desc) {

				service.addTodo((String)model.get("name"), desc, new java.util.Date(), false);

				return "redirect:/list-todos";		

			}
			
		
 
b. Delete Operation  ::

	1. JSP ::
	
		. In the list-todos.jsp we are passing todo list by putting it into a model.
		. Iterating over this list 'todo' using forEach tag of tagLib which is referenced with 'c' here.  
		. Also adding one more column to this table for Delete button with URL to delete it on basis of ID.
		. Here we are performing delete by calling controllwe which have mappign for "/deleteTodo?id=${todo.getId()}".
		. Here for button we have used a bootstrap class btn btn-warning to style it.
		
		<c:forEach items="${todos}" var="todo">
			<tr>
				<td>${todo.desc}</td>
				<td>${todo.targetDate}</td> 
				<td>${todo.isDone()}</td>
				<td><a href = "/deleteTodo?id=${todo.getId()}" class="btn btn-warning"> Delete </a></td>
			</tr>
		</c:forEach>
	
	
	2. Controller ::
	
		. Deleting a todo usign a custom method deleteTodo on basis of id which we have passed through JSP
	
		
		@RequestMapping(value="/deleteTodo",method= RequestMethod.GET)
		public String deleteTodo(@RequestParam int id) {
			service.deleteTodo(id);
			return "redirect:/list-todos"; 
		}
 
