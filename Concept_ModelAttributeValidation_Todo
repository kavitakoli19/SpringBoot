1. Command Bean or ModelAttribute or Form backing bean
2. Sending data from bean to form 
3. Sending data from form to bean
4. Adding validation to controller
5. Adding validation to form


A.
Inside form::
	<form:form method="post" modelAttribute="todo" >
		<form:label path="desc" >Description</form:label>
		<form:input path = "desc" type ="text" class="form-control" required = "required" value="sample value" /> 
		<form:button type = "submit" class = "btn btn-success"> Submit</form:button>
		<form:errors path="desc"></form:errors>
	</form:form>
    
      a. Form to bean data ::
            1. here we create a reference to bean class using the default constructor.
            2. "path" will signify the varible of class;name should match exactly with the class vaiable.
                value given in text box will be assigned to respective variable of the object.
      b. Validation ::
            1. required = "required"  :: This files is mandatory check 
            2. <form:errors path="desc"></form:errors>  :: This will show up the error message on the form;which is
            associated with the class varibale desc under tag  @Size(min=10,message="Please enter at least 10 chars....")
        


Inside Controller::POST::
	public String addTodoPost(ModelMap model,@Valid Todo todo , BindingResult result) {
		if(result.hasErrors()) {
			return "addtodo";
		}
  
   a. Form to bean data ::
          1. Here we receive the todo object created in the JSP form tag above with the desc attribute value filled in.
   
   b. Validation ::
          1. @Valid is used to indicate that we need to validate this object.
          2. BindingResult will give result of the validation. either sucess or failure
  
  
Inside Controller::GET::

@RequestMapping(value="/addtodo",method = RequestMethod.GET)
	public String addToDoGet(ModelMap model) {
		
		model.addAttribute("todo", new Todo(0, (String)model.get("name"), "test", new java.util.Date(),false));
    
    

 a. Bean to form data ::
          1. This is a GET call.
          2. We are adding a new object to ModelMap which will be available on the form in todo modelAttribute
          3. SO passing data from bean to form
          4. to validate this remove sample value from desc text box in jsp and look for "test" as a description when this page
          will render.


 

Inside Bean class::

public Todo(){
}

 @Size(min=10,message="Please enter at least 10 chars....")
    private String desc;
    
      a. Form to bean data ::
          1. When we create a form with modelAttribute for bean we need to have a default constructor.This is mandatory.
          2. As first a object will be created and respective desc will be added to it.
          3. Without this we will get nullPointer exception.
          
          
       b. Validation :: 
           1. javax.validation.constraints package have many validations availble. One of thos is @Size
           2. Form path=desc would map this annotion for validation condition and <form:erros > will display 
              given message as an error message.





