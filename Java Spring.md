# Spring Basics

## IoC containers
- The Spring Framework's IoC container core is defined in two packages: 
- org.springframework.beans and org.springframework.context.BeanFactory (org.springframework.beans.factory.BeanFactory) 
- ApplicationContext (org.springframework.context.ApplicationContext) are two important interfaces that provide a basis for IoC containers.

## BeanFactory
- provides the configuration framework and basic functionality and takes care of bean instantiation and wiring

## ApplicationContext
- ApplicationContext can also take care of bean instantiation and wiring

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
