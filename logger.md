# Logging in Spring Boot and Java

## Common Logging Frameworks
- SLF4J (Simple Logging Facade for Java)
- Logback (Default in Spring Boot)
- Log4j2
- Java Util Logging

## Spring Boot Default Logging
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyService {
    private static final Logger logger = LoggerFactory.getLogger(MyService.class);
    
    public void doSomething() {
        logger.debug("Debug message");
        logger.info("Info message");
        logger.warn("Warning message");
        logger.error("Error message");
    }
}
```

## Logging Configuration
```properties
# application.properties
logging.level.root=INFO
logging.level.com.myapp=DEBUG
logging.file.name=myapp.log
```

## Best Practices
1. Use appropriate log levels
2. Include contextual information
3. Log exceptions properly
4. Use structured logging
5. Configure log rotation

## Common Log Levels
- TRACE - Most detailed information
- DEBUG - Debugging information
- INFO - General information
- WARN - Warning messages
- ERROR - Error messages
- FATAL - Critical failures

## Logback Configuration in Spring Boot

### Significance of logback.xml
- Default logging implementation in Spring Boot
- Provides flexible configuration options
- Supports automatic reloading
- Offers pattern layouts for log formatting
- Enables log file rotation and compression

### Basic logback.xml Example
```xml
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    
    <root level="INFO">
        <appender-ref ref="CONSOLE" />
    </root>
</configuration>
```

### Key Features
1. Multiple appenders support
2. Rolling file policies
3. Conditional processing
4. Variable substitution
5. Automatic scan for changes