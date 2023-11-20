# Slf4j with Logback in spring boot

## 1. Introduction

Logging is a crucial aspect of software development for monitoring, debugging, and understanding the flow of an application. This repository focuses on using Logback, a popular logging framework, in a Spring Boot environment.

## 2. The Basics of Logging in Spring Boot

### 2.1. the significance of logging in software development

#### Debugging and Troubleshooting:

- **Error Identification**: When an issue occurs, logs provide details about the context
- **Stack Traces**:shows the series of method calls that were invoked leading up to the point where the exception occurred.

#### Monitoring Application Behavior:

- **Runtime Information**: understanding how the application behaves during execution.
- **Performance Metrics**:identify bottlenecks and optimize critical sections of the code.

#### Auditing and Compliance

- **Security Audits**: tracking user authentication, authorization attempts, and other security-related events.

#### User Support and Issue Resolution:

- **User Activity**: Logs can capture user interactions and activities within the application -**Issue Resolution**: Use logs to diagnose and resolve user-reported issues effectively.

#### Historical Analysis:

- **Trend Analysis**: Logs provide a historical record of an application's behavior over time. -**Root Cause Analysis**: When investigating incidents or outages.

#### Communication and Collaboration:

- **Communication Tool**: Logs serve as a communication tool between development, operations, and support teams. They provide a common ground for discussing and resolving issues. -**Collaboration**: Teams can share log files to collaborate on problem-solving, especially in distributed or microservices architectures.

#### Continuous Improvement:

- **Feedback Loop**: The information gathered from logs can be used as feedback for continuous improvement.

### 2.2. the role of logging frameworks

**Logging frameworks** like **SLF4J** play a vital role by providing a standardized and flexible logging infrastructure. They abstract the complexities of logging, allowing developers to focus on writing code while ensuring that the application's behavior is transparent and accessible for monitoring and diagnostics. The role of logging frameworks in standardizing and enhancing the logging process is fundamental to building robust and maintainable software systems.

### 2.3. Understanding Logging Levels

In the world of logging, not all messages are created equal. Messages are categorized by severity or importance, known as logging levels. Spring Boot supports the standard levels, which are:

> **ERROR**: Denotes that something failed, and the application might not be able to continue running.
>
> **WARN**: Indicates a potential problem that might not immediately affect functionality but warrants attention.
>
> **INFO**: Provides general information about the application’s operation. Typically used to confirm things are working as expected.
>
> **DEBUG**: Offers detailed insights for developers to diagnose issues or understand the flow.
>
> **TRACE**: Gives more granular details than DEBUG, often including iterative or repetitive processes.
>
> Each level is inclusive of the levels above it. For instance, if you set the level to WARN, you’ll also see ERROR messages, but not INFO, > DEBUG, or TRACE.

## 3. Dependencies

To use SLF4J in a Spring Boot application, ensure the following dependencies are added to your project:

```bash
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.20</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

## 4. Configuring Logging in application.properties

Update application properties in src/main/resources/application.properties:

Spring Boot allows developers to configure the logging system using the application.properties (or application.yml) file. Here are some common configurations:

- **Setting Global Logging Level**: To set a base level for all loggers:

```bash
logging.level.root=WARN
```

Setting Specific Logging Level: To define a specific level for a particular package or class:

```bash
logging.level.org.springframework.web=INFO # log messages with severity levels INFO and higher (e.g., WARN, ERROR) from the Spring Web package (and its subpackages) will be recorded.
logging.level.com.logging.toturial.controllers=DEBUG
logging.level.org.hibernate=ERROR # Log messages with lower severity levels such as WARN, INFO, or DEBUG will be ignored
```

In the provided example, Hibernate-related log messages will only include errors, while Spring Web-related log messages will include information at the INFO level and above.

Log File Output: By default, logs are printed to the console. If you want them saved to a file:

```bash
logging.file.name=logs/spring-boot-logging.log
```

Log File Rotation: For larger applications, logs can grow rapidly. To manage size, Spring Boot can rotate logs:

```bash
logging.file.max-size=10MB
logging.file.max-history=10
```


## 5. Logging Annotations by Lombok
### What is Lombok?

Lombok is a compile-time annotation processor. Instead of you writing repetitive code or relying on your IDE to generate it, Lombok provides annotations to instruct the compiler to generate the code on your behalf.

While Lombok offers various annotations for diverse tasks, like @Data for getters, setters, and other common methods, we'll focus on the logging annotations:

**@Slf4j**: This is the most commonly used logging annotation for Spring Boot applications. When applied to a class, it automatically creates a static SLF4J logger instance named log, targeting the SLF4J logging facade.

```bash
@Slf4j
public class MyService {
    public void someServiceMethod() {
        log.info("Service method called using @Slf4j");
    }
}
```

## 6. Best Practices with @Slf4j and Logging

Logging effectively is as much about technique as it is about the tools. While @Slf4j eliminates boilerplate and simplifies logger instantiation, it's vital to understand and adhere to logging best practices to make the most of it.

### 6.1. Log Meaningful Messages

Ensure that each log message provides context and is clear enough for someone unfamiliar with the code to understand. Ambiguous messages like “Error occurred” should be avoided.

```bash @Slf4j
public class PaymentService {
    public void processPayment(Payment payment) {
        if (payment == null) {
            log.error("Payment processing failed due to null payment object.");
        }
        // ...
    }
}
```

### 6.2. Use Appropriate Logging Levels

Misusing log levels can result in missed critical information or log bloat. Ensure you’re using the right level:

**ERROR**: For serious issues that may prevent the application from continuing.
**WARN**: For potential problems that don’t halt operation.
**INFO**: General operational messages about the application state ( such as the successful start of app or the completion of important tasks)
**DEBUG**: Messages useful for debugging, but too verbose for general logs.
**TRACE**: Very detailed messages, typically used for intricate debugging.

### 6.3. Avoid Logging Sensitive Information

Never log sensitive information like passwords, credit card numbers, or personally identifiable information (PII). This is a security best practice and, in many jurisdictions, a legal requirement.

### 6.4. Use Parameterized Logging

Instead of string concatenation, utilize parameterized logging provided by SLF4J. This approach is efficient and can prevent unnecessary string creation.

```bash
String orderId = "O12345";
log.info("Processing order with ID: {}", orderId);
```

### 6.5. Avoid Logging Inside Tight Loops

Logging inside loops, especially tight loops, can slow down an application significantly and generate enormous log files. Be judicious with logging inside loops, especially at DEBUG or TRACE levels.

### 6.6. Stay Consistent

Maintain consistency in logging patterns across your application. It aids in readability and ensures automated tools can parse logs effectively.

## 7. References

[SLF4J Official Documentation](https://www.slf4j.org/docs.html)

[Project Lombok Documentation](https://projectlombok.org/features/all)

[Spring Boot Logging Guide](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-logging)
