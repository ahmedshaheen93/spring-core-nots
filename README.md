# spring-core-nots
## Instantiating beans
###	1-Constructor
  		- Default Constructor    	---> no constructor written on bean so it will call parent Constructor Object()
  		- No arg Constructor 		---> UserServiceImpl()
  		- Overloaded Constructor 	---> UserServiceImpl(Object o)

###	2-Static factory method
  		- Static method inside Class
  			- which can call the write contractor and instantiate the bean
###	3-factory method
  		- non static method inside a factory class
  			- which can call the write contractor and instantiate the bean
  			- when you define the bean you should specify
  				what is the factory-bean class , what is the factory-method
# Dependency Injection
