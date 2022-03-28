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
  ### Webflux
  
  ### Web requests handling
  A web application needs three levels of support for serving HTTP web requests
  
  1. HttpHandler: An interface that is an abstraction of a request/response handler over different HTTP server APIs, such as Netty or Tomcat
  2. WebHandler: Provides support for user sessions, request and session attributes, a locale and principal for the request, form data, and so on
  3. Codecs: (Encoder, Decoder, HttpMessageWriter, HttpMessageReader, and DataBuffer) for the serialization and deserialization of content at both the server and client level for the request and response

# Secruity
  ## Access Token 
  ### JavaScript Object Notation (JSON) Web Token (JWT)
    - Spring Security provide the servlet pre-filters that are processed before the request reaches **DispatcherServlet**
  ### Opaque tokens
    - Spring Security also provides support for opaque tokens
    - similar to JWTs. The main difference between them is how information is read from the token
  ### Security Filter Chain
    1. WebAsyncManagerIntegrationFilter
    2. SecurityContextPersistenceFilter
    3. HeaderWriterFilter
    4. CorsFilter
    5. CsrfFilter
    6. LogoutFilter
    7. BearerTokenAuthenticationFilter
    8. RequestCacheAwareFilter
    9. SecurityContextHolderAwareRequestFilter
    10. AnonymousAuthenticationFilter
    11. SessionManagementFilter
    12. ExceptionTranslationFilter
    13. FilterSecurityInterceptor
    14. Finally reaches the controller
  ### JWT Auth Implementation
  0. Add the **spring-boot-starter-spring-security** dependency into the project
  1. Create a **SecurityConfig** class class, which extends **WebSecurityConfigurerAdapter**
  2. Add the annotation **@EnableWebSecurity** to this class
  3. Override the **configure(HttpSecurity http)** method
  4. Register 2 custom filters by using the addFilter method (**LoginFilter** and **JwtAuthenticationFilter**) 
      - LoginFilter for issuing the JWT when the user entered the correct username and password
      - JwtAuthenticationFilter for verifing the JWT in the Authorization header. Bearer ${JWT token}
  
  ``` java
  
  import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
  import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
  import org.springframework.security.config.annotation.web.builders.HttpSecurity;
  import org.springframework.http.HttpMethod;

  @EnableWebSecurity
  public class SecurityConfig extends WebSecurityConfigurerAdapter {
        
      // Expose the bean to be used in the AuthenticationManager.authenticate()
      @Bean
      @Override
      protected UserDetailsService userDetailsService() {
        return userService;
      }
  
      @Override
      protected void configure(HttpSecurity http) throws Exception {
  
          // sign in url does not require authentication
          String SIGN_UP_URL = "/api/v1/users";

          http.authorizeRequests()
              .antMatchers(HttpMethod.POST, SIGN_UP_URL).permitAll()
              .anyRequest().authenticated()
              .and()
              // Register the custom filters here
              .addFilter(new LoginFilter(super.authenticationManager(), mapper))
              .addFilter(new JwtAuthenticationFilter(super.authenticationManager()))
              .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
      }

  }
  ```
  
  5. Create the **LoginFilter** class which extends the **UsernamePasswordAuthenticationFilter**
  6. Override the **attemptAuthentication()** and **successfulAuthentication()**
  
  ``` java
  
  public class LoginFilter extends UsernamePasswordAuthenticationFilter {

    private final AuthenticationManager authenticationManager;
  
    // JwtManager allows you to create a JWT based on the given org.springframework.security.core.userdetails.User.
    private final JwtManager tokenManager;
  
    private final ObjectMapper mapper;

    public LoginFilter(AuthenticationManager authenticationManager, JwtManager tokenManager,
        ObjectMapper mapper) {
    
      // Token login URL
      String TOKEN_URL = "/api/v1/auth/token";
  
      this.authenticationManager = authenticationManager;
      this.tokenManager = tokenManager;
      this.mapper = mapper;
      super.setFilterProcessesUrl(TOKEN_URL);
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest req,
        HttpServletResponse res) throws AuthenticationException {
  
      if (!req.getMethod().equals(HttpMethod.POST.name())) {
        throw new MethodNotAllowedException(req.getMethod(), List.of(HttpMethod.POST));
      }
      try (InputStream is = req.getInputStream()) {
  
        // SignInReq is a Plain Old Java Object (POJO) that contains the username and password fields.
        // Created on our own
        SignInReq user = new ObjectMapper().readValue(is, SignInReq.class);
  
        // We simply use it to authenticate by passing the username, password, and authorities
        // authorities is optional
        return authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(
                user.getUsername(),
                user.getPassword(),
                Collections.emptyList())
        );
      } catch (IOException e) {
        throw new RuntimeException(e);
      }
    }

    // Once the login is successful, we need to return a JWT in response.
    @Override
    protected void successfulAuthentication(HttpServletRequest req,
        HttpServletResponse res,
        FilterChain chain,
        Authentication auth) throws IOException {
  
      User principal = (User) auth.getPrincipal();
  
      // Create the JWT token
      String token = tokenManager.create(principal);
      
      // SignedInUser is a POJO that contains the username and token fields
      // Created on our own
      SignedInUser user = new SignedInUser().username(principal.getUsername()).accessToken(token);
  
      res.setContentType(APPLICATION_JSON_VALUE);
      res.setCharacterEncoding("UTF-8");
      res.getWriter().print(mapper.writeValueAsString(user));
      res.getWriter().flush();
    
      // The client receives the username and token as a response after successful authentication 
      // then can then use this token in the Authorization header by prefixing the token value with "Bearer".
    }
  }
  
  ```
  
  7. Create the **JwtAuthenticationFilter** class which extends BasicAuthenticationFilter to verify the JWT
  
  ``` java
  
  public class JwtAuthenticationFilter extends BasicAuthenticationFilter {

    public JwtAuthenticationFilter(AuthenticationManager authenticationManager) {
      super(authenticationManager);
    }

    @Override
    protected void doFilterInternal(HttpServletRequest req, HttpServletResponse res,
        FilterChain chain) throws IOException, ServletException {
      String header = req.getHeader(AUTHORIZATION);
      if (Objects.isNull(header) || !header.startsWith(TOKEN_PREFIX)) {
        chain.doFilter(req, res);
        return;
      }

      Optional<UsernamePasswordAuthenticationToken> authentication = getAuthentication(req);
      authentication.ifPresentOrElse(e -> SecurityContextHolder.getContext().setAuthentication(e),
          SecurityContextHolder::clearContext);
      chain.doFilter(req, res);
    }

    private Optional<UsernamePasswordAuthenticationToken> getAuthentication(
        HttpServletRequest request) {
      String token = request.getHeader(AUTHORIZATION);
      if (Objects.nonNull(token)) {
        DecodedJWT jwt = JWT.require(Algorithm.HMAC512(SECRET_KEY.getBytes(StandardCharsets.UTF_8)))
            .build()
            .verify(token.replace(TOKEN_PREFIX, ""));
        String user = jwt.getSubject();
        @SuppressWarnings("unchecked")
        List<String> authorities = (List) jwt.getClaim(ROLE_CLAIM);
        if (Objects.nonNull(user)) {
          return Optional.of(new UsernamePasswordAuthenticationToken(
              user, null, Objects.nonNull(authorities) ? authorities.stream().map(
              SimpleGrantedAuthority::new).collect(Collectors.toList()) : Collections.emptyList()));
        }
      }
      return Optional.empty();
    }
  }
  
  
  ```
