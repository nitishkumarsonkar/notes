# Singleton Pattern

The Singleton pattern ensures a class has only one instance and provides a global point of access to it.

## Key Characteristics
- Private constructor
- Static instance
- Global access point

## Common Use Cases
- Database connections
- Configuration settings
- Logger instances
- Thread pools

## Example Implementation (Java)
```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## Advantages
- Controls access to shared resources
- Ensures single instance
- Lazy initialization possible

## Disadvantages
- Can make unit testing difficult
- Can be considered an anti-pattern
- Global state can be problematic

## Role of the Singleton Pattern in Spring Boot

In Spring Boot, the default scope for beans is **singleton**. This means that the Spring container creates a single instance of each bean, which is then shared across the entire application context. This is analogous to the Singleton design pattern, where only one instance of a class is allowed.

**Key Points:**
- **Resource Efficiency:** A single instance is created and reused, reducing overhead.
- **Consistency:** Ensures that every component that depends on a bean gets the same instance, which is particularly useful for stateless services.
- **Performance:** Beans are initialized only once, which can lead to faster startup times and reduced memory usage.
- **Dependency Injection:** Spring manages bean creation and injection, so you can rely on the singleton behavior without implementing manual checks.

While singletons are ideal for many components (like service and repository layers), Spring also supports other scopes (such as prototype, request, and session) for cases where a single instance is not appropriate.