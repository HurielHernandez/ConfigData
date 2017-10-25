# ConfigData

### Add Config Server to client project

```
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```
#### Add Actuator to clien project.  Allows client project to refresh and restart configurations during run time.
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-actuator</artifactId>
</dependency>
```
#### Create bootstrap.yml in resources folder and add:
```
spring:
  application:
    name: application  # name of application
  profiles:
    active: dev        # name of profile.  Examples include: dev, production, default
  cloud:    
    config:
      uri: http://localhost:8888  # url of config server where application will look for configuration files from.
      
endpoints:
  refresh:
    enabled: true     # allows application to refresh properties by sending a post request to http://application-url/refresh
  restart:
    enabled: true    # allows application to restart application context by sending a post request to http://application-url/restart

management:
  security:
    enabled: false  # allows refresh / restart to be used
```
### Add Refresh Scope annotation Main class:
```
@RefreshScope
@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}

```
Application will look for 
```/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```

### Refresh client applcation properties
If changes are made to the application.yml of an application that is running,  the client application can be refreshed in order to apply the changes while the application is running by simply sending a POST request to the client application:
``` 
$ curl -d {} http://client-url/restart
```
