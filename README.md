# spring-core-nots
## Instantiating beans
###	1-Constructor
- Default Constructor    	---> no constructor written on bean so it will call parent Constructor Object()
- No arg Constructor 		---> UserServiceImpl()
- Overloaded Constructor 	---> UserServiceImpl(Object o)

###	2-Static factory method
- Static method inside Class
 - which can call the right contractor and instantiate the bean

###	3-factory method
- non static method inside a factory class
	- which can call the right contractor and instantiate the bean
	- when you define the bean you should specify what is the factory-bean class , what is the factory-method

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
- note
  - Constructor Injection for mandatory dependencies.
  - Setter methods for optional dependencies.
  - Circular dependencies might happen when you use Constructor Injection, to avoid that use Setter method

## Autowiring
#### 1. no (default)

- means that you are responsable for wiring bean dependancies by yourself

#### 2. byName

- Autowiring by property name.
-Container Wiring a bean with the same name as the property that needs to be autowired.
```java
  public class UserServiceImpl{
    // autowird bean
    // container will search for a bean with the same name and inject it here
    private  UserRepo repo;
  }
```
```xml
<beans>
  <bean id="userRepo" class="com.shaheen.repo.UserRepo" />
  <bean id="userService" class="com.shaheen.service.UserServiceImpl" autowire="byName"/>
</beans>
```

#### 3. byType

- Autowiring by property type
- Container Wiring if exactly one bean of the property type exists in the container.
- If more than one exists, a fatal exception is thrown, which indicates that you may not use byType autowiring for that bean.
- If there are no matching beans, nothing happens (the property is not set).

```java
public class UserController{
  // UserService is the parent interface for UserServiceImpl class
  // the container will search for a bean with the same type and inject it here
  private UserService userService;
}
```
```xml
<beans>
  <bean id="userRepo" class="com.shaheen.repo.UserRepo" />
  <bean id="userService" class="com.shaheen.service.UserServiceImpl" autowire="byName"/>
  <bean id="UserController" class="com.shaheen.controller.UserController" autowire="byType">
</beans>
```

#### 4. constructor
- Autowiring by property type in constructor arguments
- Container Wiring byType but applies to constructor arguments.
- If there is not exactly one bean of the constructor argument type in the container, a fatal error is raised.

```java
public class UserController{
  // UserService is the parent interface for UserServiceImpl class
  private UserService userService;
   // the container will search for a bean with the same type and inject it here
  public UserController(UserService userService){
    this.userService = userService;
  }
}
```
```xml
<beans>
  <bean id="userRepo" class="com.shaheen.repo.UserRepo" />
  <bean id="userService" class="com.shaheen.service.UserServiceImpl" autowire="byName"/>
  <bean id="UserController" class="com.shaheen.controller.UserController" autowire="constructor">
</beans>
```