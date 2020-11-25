# Spring Properties file import

## Register a Properties file via Java Annotations

1. `@PropertySource` is to be used in conjunction with java-based configuration and the `@Configuration` annotation.

``` java
@Configuration
@PropertySource("classpath:foo.properties")
public class PropertiesWithJavaConfig {
    //...
}
```
2. Defining Multiple Property location
   
``` java
@PropertySources({
    @PropertySource("classpath:foo.properties"),
    @PropertySource("classpath:bar.properties")
})
public class PropertiesWithJavaConfig {
    //...
}
```

## Using/Injecting Properties

1. Injecting a property with `@Value` annotation
   
``` java
@Value("${jdbc.url}")
private String jdbcUrl;
```

2. We can also specify a default value for the property

``` java
@Value( "${jdbc.url:aDefaultUrl}" )
private String jdbcUrl;
```

3. We can obtain the value of a property using the EnvironmentAPI

``` java
@Autowired
private Environment env;
...
dataSource.setUrl(env.getProperty("jdbc.url"));
```

## Properties with Spring Boot

### application.properties under /src/main/resources
Boot applies its typical convention over configuration approach to property files. This means that we can simply put an application.properties file in our src/main/resources directory, and it will be auto-detected. We can then inject any loaded properties from it as normal **without explicitly register a PropertySource or even provide a path to a property file.**

### Dev and Prod environment using different properties

1. Setting up application-dev.properties and application-prod.properties
   - For the dev environment you can use an in-memory database like H2. Create a new file in src/main/resources/ directory named **application-dev.properties**. Inside the file there is the configuration of the in-memory database:
   ``` 
   spring.datasource.url=jdbc:h2:mem:test
   spring.datasource.driverClassName=org.h2.Driver
   spring.datasource.username=sa
   spring.datasource.password=
   ```

   - For the prod environment we will connect to a "real" database for example postgreSQL. Create a new file in src/main/resources/ directory named **application-prod.properties**. Inside the file there is the configuration of the postgreSQL database:
   ``` 
   spring.datasource.url= jdbc:postgresql://localhost:5432/yourDB
   spring.datasource.username=postgres
   spring.datasource.password=secret
   ```
2. Setting up default application.properties
   - In your default application.properties file you are now able to set which profile is activated and used by Spring-Boot. Just set one attribute inside:
   `spring.profiles.active=dev`
   or
   `spring.profiles.active=prod`
   - Important is that the part after - in application-dev.properties is the identifier of the file.


## Hierarchical Properties
If we have properties that are grouped together, we can use `@ConfigurationProperties` annotation, whcih will map properties hierarchies into java objects graphs.

Property file:
```
database.url=jdbc:postgresql:/localhost:5432/instance
database.username=foo
database.password=bar
```

Using properties under `database`:
``` java 
@ConfigurationProperties(prefix = "database")
public class Database {
    String url;
    String username;
    String password;
 
    // standard getters and setters
}
```