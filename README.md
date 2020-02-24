# SpringBoot
Spring Boot concepts

- Beans
  - @Component : Generic, covers all of other beans
  - @Controller : Should be used for class handling web requests
  - @Service : Should be used for classes writing business logic like say checking user credentials
  - @Repository : Used for classes whose main purpose is to store the data

- Dependency Injection :
  - @Autowired and Beans mentioning through their annotation....Prevents hard coding and hence helps in future enhancemnets
  
- Annotations to handle web parameters:
  - @RequestParam 
  - ModelMap : its ot annotation....is used for putting parameters as key value pairs
  - @RequestMapping(value='/login',method=RquestMethod.POST/GET)
  
- Other annotations:
  - @ResponseBody : is used when we have not defined jsp pages......strijng returned through method is shown on web page.
  - @ComponentScan : is used to gather all beans defined in the same package.
  
