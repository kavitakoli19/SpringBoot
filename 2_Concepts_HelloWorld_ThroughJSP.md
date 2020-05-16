# Concept ::
  - In earlier application we seen mapping a URL to a @RequestMapping and returning a response in the form of a String
  as a web page body with the help of @ResponseBody annotation.
  - In this application we will see how to retun a JSP page as a reponse to the URL request.
  
 # Changes done for this application
  - Removed @ResponseBody as we are no more sending a string as response on web page.
  - Added @RequestParam this is a parameter which we will pass as part of a request.
      - Name of the parameter should match with the name of parameter used in URL.
  - Added a parameter ModelMap; It can be used to keep the data in a map which can be passed to JSP pages or UI tool
    and also can be passed across the chained method calls.
  - In application.properties file added below properties : 
      - spring.mvc.view.prefix=/WEB-INF/jsp/ :: where to look for UI pages/classes.
      - spring.mvc.view.suffix=.jsp  :: what is the extension to suffix with.
  - Added a JSP page at above mentioned location
      - In this page attribute which was added to mdelMap with supplied name "name" is referred with belw syntax
        - ${name}
  
   
  
  
 
 # Annotations ::
  - ModelMap : Used in Building model data for use with UI tools. Supports chained calls and generation of model attribute names.
  - @RequestParam : 
  
  
  # How to execute :: 
  http://localhost:8080/login?name=DummyName
  
