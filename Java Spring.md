# IoC containers
- The Spring Framework's IoC container core is defined in two packages: 
1. org.springframework.beans and org.springframework.context.BeanFactory (org.springframework.beans.factory.BeanFactory) 
2. ApplicationContext (org.springframework.context.ApplicationContext) are two important interfaces that provide a basis for IoC containers.

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
    @Bean(initMethod = "init", destroyMethod = "destroy", name = {
        "sampleBean",
        "sb"
    })
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
