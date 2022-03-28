# Spring Basics

## IoC containers
- The Spring Framework's IoC container core is defined in two packages: 
- org.springframework.beans and org.springframework.context.BeanFactory (org.springframework.beans.factory.BeanFactory) 
- ApplicationContext (org.springframework.context.ApplicationContext) are two important interfaces that provide a basis for IoC containers.

## BeanFactory
- provides the configuration framework and basic functionality and takes care of bean instantiation and wiring

## ApplicationContext
- ApplicationContext can also take care of bean instantiation and wiring
- provides more enterprise-specific functionalities, as follows:
  - Integrated life cycle management
  - Automatic registration of **BeanPostProcessor** and **BeanFactoryPostProcessor**
  - Internationalization (message resource handling) with easy access to MessageSource
  - Events publication using a built-in ApplicationEvent
  - Provides WebApplicationContext, an application layer specific context for web applications

## Configuration metadata
- tells ApplicationContext to know about what beans to instantiate, assemble, and configure
- can be represented in three ways: through XML configuration, Java annotations, and Java Code

```java
public class SampleBean {
    public void init() { // initialization logic 
    }
    public void destroy() { // destruction logic 
    }
    // bean code
}

public interface BeanInterface { // interface code 
}

public class BeanInterfaceImpl implements BeanInterface {
    // bean code
}

@Configuration
public class AppConfig {
    // SampleBean has two bean names: sampleBean and sb
    @Bean(initMethod = "init", destroyMethod = "destroy", name = {"sampleBean", "sb"})
    @Description("Demonstrate a simple bean")
    public SampleBean sampleBean() {
        return new SampleBean();
    }
    @Bean
    public BeanInterface beanInterface() {
        return new BeanInterfaceImpl();
    }
}
```

### @Autowired
- allows you to define the configuration part in a bean's class itself instead of writing a separate configuration class annotated with @Configuration
- can be applied to a field
- only work if there is no constructor or setter method to inject the dependent bean

```java
@Component
public class CartService {
    private CartRepository repository;
    private ARepository aRepository;
    private BRepository bRepository;
    private CRepository cRepository;
    
    @Autowired // member(field) based auto wiring
    private AnyBean anyBean;
    
    @Autowired // constructor based autowired
    public CartService(CartRepository cartRepository) {
        this.repository = repository;
    }
    
    @Autowired // Setter based auto wiring
    public void setARepository(ARepository aRepository) {
        this.aRepository = aRepository;
    }
    
    @Autowired // method based auto wiring
    public void xMethod(BRepository bRepository, CRepository cRepository) {
        this.bRepository = bRepository;
        Learning the basic concepts of the Spring Framework 43
        this.cRepository = cRepository;
    }
}
```
### @Qualifier
- @Qualifier helps you to resolve which type should be used when multiple beans are available for injection

``` java

@Configuration
public class AppConfig {
    
    @Bean
    @Qualifier("cartService1")
    public CartService cartService1() {
        return new CartServiceImpl1();
    }

    @Bean
    @Qualifier("cartService2")
    public CartService cartService2() {
        return new CartServiceImpl2();
    }
}

@Controller
public class CartController {
    @Autowired
    private CartService service1;
    @Autowired
    private CartService service2;
}


```
### @Primary
- The @Primary annotation allows you to set one of the type's beans as the default

``` java
@Configuration
public class AppConfig {
    @Bean
    @Primary
    public CartService cartService1() {
        return new CartServiceImpl1();
    }
    @Bean
    public CartService cartService2() {
        return new CartServiceImpl2();
    }
}
@Controller
public class CartController {
    @Autowired
    private CartService service;
}
```

### @Value
- Spring supports the use of external property files: <xyz>.properties or <xyz>.yml.
- Now, you want to use the value of any property in your code. You can achieve this using the @Value annotation.
   
```java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {}

@Controller
public class CartController {
    @Value("${default.currency}")
    String defaultCurrency;
} 
    
```
## AOP
- Example of time tracking of a annotated method
    
```java
// Define a new annotation
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface TimeMonitor {}



@Aspect
@Component
public class TimeMonitorAspect {

    // Aspects insert additional logic during the execution of the program at a certain point. This point is known as the Join point
    // @Around


    // Pointcut determines whether the Advice needs to be executed or not
    // @annotation(com.packt.modern.api.TimeMonitor)


    // The Advice is an action taken by the Aspect at a specific time (Joinpoint)
    // logTime method

    @Around("@annotation(com.packt.modern.api.TimeMonitor)")
    public Object logTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + "takes: " + executionTime + "ms ");
        return proceed;
    }
}



class Test {

    @TimeMonitor
    public void performSomeTask() {
        // Business Logic
    }
}  
    
    
```

## Flow of a user request in Spring MVC for the REST controller
    
1. The user sends the HTTP request, which is received by **DispatcherServlet**.
2. **DispatcherServlet** passes the baton to **HandlerMapping**. **HandlerMapping** does the job of finding the correct controller for the requested URI and passes it back to **DispatcherServlet**.
3. **DispatcherServlet** then makes use of **HandlerAdaptor** to handle Controller.
4. **HandlerAdaptor** calls the appropriate method inside Controller.
5. Controller then executes the associated business logic and forms the response.
6. Spring makes use of the marshaling/unmarshalling of request and response objects for JSON/XML conversion from Java and vice versa.

  
  
# API Implementation 

  ## Database Migration 
  - Can use Flyway
  - This helps maintain the database and maintains a database changes history that allows rollbacks, version upgrades, and so on
  
## Data persistence
### @Entity

  ``` java
  
  import javax.persistence.Basic;
  import javax.persistence.CascadeType;
  import javax.persistence.Column;
  import javax.persistence.Entity;
  import javax.persistence.FetchType;
  import javax.persistence.GeneratedValue;
  import javax.persistence.Id;
  import javax.persistence.JoinColumn;
  import javax.persistence.JoinTable;
  import javax.persistence.OneToMany;
  import javax.persistence.OneToOne;
  import javax.persistence.Table;
  
  @Entity
  @Table(name = "cart")
  public class CartEntity {

    @Id
    @GeneratedValue
    @Column(name = "ID", updatable = false, nullable = false)
    private UUID id;

    @OneToOne
    @JoinColumn(name = "USER_ID", referencedColumnName = "ID")
    private UserEntity user;

    @ManyToMany(
        cascade = CascadeType.ALL
    )
    @JoinTable(
        name = "CART_ITEM",
        joinColumns = @JoinColumn(name = "CART_ID"),
        inverseJoinColumns = @JoinColumn(name = "ITEM_ID")
    )
    private List<ItemEntity> items = Collections.emptyList();
  
    // Omited the setter and getter functions
  }
  ```
  #### @one-to-one 
  - mapping the Cart entity to the User Entity
  
  #### @many-to-many 
  - mapping the Cart entity to Item Entity. The ItemEntity list is also associated with @JoinTable, because you are using the CART_ITEM join table to map the cart and product items based on the CART_ID and ITEM_ID columns in their respective tables.
  
  
  ``` java
  
  @Entity
  @Table(name = "user")
  public class UserEntity {
    ...
  
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY, orphanRemoval = true)
    private List<CardEntity> cards;

    @OneToOne(mappedBy = "user", fetch = FetchType.LAZY, orphanRemoval = true)
    private CartEntity cart;

    @OneToMany(mappedBy = "userEntity", fetch = FetchType.LAZY, orphanRemoval = true)
    private List<OrderEntity> orders;
  
    ...
  }
  ```
  
  #### fetch = FetchType.LAZY
  - means the user's cart will be loaded only when asked for explicitly
  
  #### orphanRemoval = true
  - you want to remove the cart if it is not referenced by the user
  
# Reactive API
  ## Reactive Core
  ### Web requests handling
  A web application needs three levels of support for serving HTTP web requests
  
  1. HttpHandler: An interface that is an abstraction of a request/response handler over different HTTP server APIs, such as Netty or Tomcat
  2. WebHandler: Provides support for user sessions, request and session attributes, a locale and principal for the request, form data, and so on
  3. Codecs: (Encoder, Decoder, HttpMessageWriter, HttpMessageReader, and DataBuffer) for the serialization and deserialization of content at both the server and client level for the request and response
