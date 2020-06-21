
# Scope of a requestParam::

  - In any request the requestParams we receive lasts for that request only.
  - So requestParam that we add in ModelMap can be accessible only within the same request.
  - If we try to access the requestParam using model.get("name") in the request other than its originating
     request then we will get null value in return as its default scope is "request" scope.
  - To make any requestParam stored in an modelMap to be available across requests we have a annotation as below;
     which can be written over a Cotroller class as :
      
                @SessionAttributes("name")
                
  - So in this example requestParam name is having a session scope so we are able to access it in addToDoPost() method.
     
