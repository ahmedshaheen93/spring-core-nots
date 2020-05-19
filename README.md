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
## Dependency Injection
### 1-Constructor Injection
####  -define bean with write contractor that takes injected beans
```java
  public class UserServiceImpl{
    // reference to injected bean
    private  UserRepo repo;
    // contractor Injection of UserRepo
    public UserServiceImpl(UserRepo repo){
      this.repo =repo;
    }
  }
```
```xml
<beans>
  <bean id="userRepo" class="com.shaheen.repo.UserRepo"></bean>
  <bean id="userService" class="com.shaheen.service.UserServiceImpl">
    <constructor-arg ref="userRepo"/>
  </bean>
</beans>
```
### 2-Factory Method Injection
#### -define a factory bean with a factroy method that tacks dependacy as arg 
```java
//factory class
public class ServiceFactory{
  // factory method
  public UserService createUserService(UserRepo repo){
    return new UserServiceImpl(repo);
  }
}
```
```xml
<beans>
  <bean id="factory" class="com.shaheen.service.ServiceFactory"></bean>
  <bean id="userRepo" class="com.shaheen.repo.UserRepo"></bean>
  <bean id="userService" factory-bean="factory" factory-method="createUserService">
    <constructor-arg ref="userRepo"/>
  </bean>
</beans>
```
### 3-Setter Injection
#### -define a set method that tacks dependacy as arg inside
```java
  public class UserServiceImpl{
    // reference to injected bean
    private  UserRepo repo;
    // setter Injection of UserRepo
    public void setUserRepo(UserRepo repo){
      this.repo =repo;
    }
  }
```
```xml
<beans>
  <bean id="userRepo" class="com.shaheen.repo.UserRepo"></bean>
  <bean id="userService" class="com.shaheen.service.UserServiceImpl">
    <property name="repo" ref="userRepo"/>
  </bean>
</beans>
```