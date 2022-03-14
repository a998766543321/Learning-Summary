## Dependency Injection

What is dependency?
-- Dependencies mean that one class has a field in another class or uses a method

What is injection?
-- Injection is the assignment of an instance to a variable.

```java
public class SomeObjectA {
    
    // field
    // SomeObjectA has a dependency of SomeObjectB
    private SomeObjectB objB;

    // constructor
    // Injection of the SomeObjectB to SomeObjectA
    public SomeObjectA(SomeObjectB objB ) {
        this.objB = objB;
    }
    // method
    public void methodA() {
        objB.methodB();
    }
}
```

What is dependency injection?
-- Dependency injection is the assignment of an instance of a class that inherits from an interface or abstract class.

For example, Java has an interface called 'List'. You cannot assign an instance to it unless it is a class that implements the interface. If you want to inject an 'ArrayList' class that implements the 'List' interface, write code similar to the following:

```java
List<Object> list = new ArrayList<>();
```
An interface or abstract class can have multiple classes that inherit from it. Classes that implement the 'List' interface include 'ArrayList' and 'LinkedList'.

If you have more than one implementation class, you need to do two things:

Which implementation class to use (dependency)

1. Insert an instance of the class into a variable (injection)
2. In other words, decide whether to inject an 'ArrayList' or a 'LinkedList' into the 'List'.
   
The combination of Dependency and Injection results in Dependency Injection.



What does Spring's DI do?
1. Look for the DI's target class
   1. When Spring starts, it looks for the class that is the target of DI. This process is called component scan.
        - @Component
        - @Controller
        - @Service
        - @Repository
        - @Configuration
        - @RestController
        - @ControllerAdvice
        - @ManagedBean
        - @Named
        - @Mapper
        - @Bean
    2. The class with these annotations is then registered in the DI container. A DI container is a container that manages DI target classes.
2. Injecting instances where they are annotated with @Autowired
3. Using DI, you can easily change the implementation class.
4. Spring's DI automatically creates and destroys instances.


Java Bean
- A class registered in a DI container is called a bean

Bean Scope
- Annotations such as @Service default to singleton scope


PRG Pattern (POST-Redirect-GET)
- The PRG pattern is to redirect to the screen transition (execute the GET method) after the POST method. The acronym for POST-Redirect-GET is called the PRG pattern. Use the PRG pattern to prevent accidental registration or renewal.

- [When not using PRG pattern]
Suppose you have an application that transitions to the screen without redirecting after registering something. Immediately after the registration operation, press the F5 key on the browser. Then the same request will be sent and the registration process will be performed again.

- [When using PRG pattern]
If you make a redirect after performing registration or other processing, the request will not be sent even if you press the F5 key. This prevents accidental operation by the user.


Form binding with the webpage
- Create a form class
- Use the @ModelAttribute attribute in the signature of the controller

```java

import java.util.Date;
import org.springframework.format.annotation.DateTimeFormat;
import lombok.Data;

@Data
public class SignupForm {
    private String userId;
    private String password;
    private String userName;

    @DateTimeFormat (pattern='dd/MM/yyyy')
    private Date birthday;
    private Integer age;
    private Integer gender;

    // You can use the code above to convert a comma delimiter, such as 1,000,000, to a numeric type.
    @NumberFormat (pattern='#,###')
    private Integer salary;
}
```


```java

@GetMapping ('/signup')
public String getSignup(Model model, Locale locale, @ModelAttribute SignupForm form) {

    // Transition to user signup screen
    return 'user/signup';
}

@PostMapping ('/signup' )
    public String postSignup(@ModelAttribute SignupForm form ) {
    log.info(form.toString());

    // Redirect to login screen
    return 'redirect:/login' ;
}

```
