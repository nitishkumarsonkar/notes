# Singleton Pattern (Creational)

The **Singleton Pattern** ensures a class has only one instance and provides a global point of access to it. It is used to control access to a single instance, often for resource management or coordination.

## Key Components
1. **Singleton Class**: The class that enforces a single instance, typically with a private constructor, a static instance variable, and a static method to access it.
2. **Client**: Uses the singleton instance via the static access method.

## Java Code Example: Chocolate Boiler

### 1. Singleton Class (ChocolateBoiler)
```java
public class ChocolateBoiler {
    // Static instance variable (eager initialization)
    private static ChocolateBoiler uniqueInstance = new ChocolateBoiler();

    private boolean empty;
    private boolean boiled;

    // Private constructor to prevent instantiation
    private ChocolateBoiler() {
        empty = true;
        boiled = false;
    }

    // Static method to get the single instance
    public static ChocolateBoiler getInstance() {
        return uniqueInstance;
    }

    public void fill() {
        if (isEmpty()) {
            empty = false;
            boiled = false;
            System.out.println("Filling the boiler with a chocolate mixture");
        }
    }

    public void drain() {
        if (!isEmpty() && isBoiled()) {
            empty = true;
            System.out.println("Draining the boiled chocolate");
        }
    }

    public void boil() {
        if (!isEmpty() && !isBoiled()) {
            boiled = true;
            System.out.println("Boiling the chocolate mixture");
        }
    }

    public boolean isEmpty() {
        return empty;
    }

    public boolean isBoiled() {
        return boiled;
    }
}
```

### 2. Client (ChocolateBoilerTest)
```java
public class ChocolateBoilerTest {
    public static void main(String[] args) {
        // Get the singleton instance
        ChocolateBoiler boiler = ChocolateBoiler.getInstance();

        // Simulate chocolate boiler operations
        boiler.fill();
        boiler.boil();
        boiler.drain();

        // Get the same instance again
        ChocolateBoiler anotherBoiler = ChocolateBoiler.getInstance();
        System.out.println("Are both boilers the same instance? " + (boiler == anotherBoiler));

        // Try operations on the same instance
        boiler.fill();
    }
}
```

## How It Works
1. **Single Instance**:
   - The `ChocolateBoiler` class uses a private constructor to prevent external instantiation.
   - A static `uniqueInstance` variable holds the single instance (eagerly initialized).

2. **Global Access**:
   - The `getInstance()` method provides the only way to access the singleton instance.

3. **State Management**:
   - The boiler maintains state (`empty`, `boiled`) and ensures operations like `fill`, `boil`, and `drain` are performed in a valid sequence.

4. **Thread Safety**:
   - This example uses eager initialization, which is thread-safe in Java. For lazy initialization, synchronization or other techniques (e.g., double-checked locking) may be needed.

## Output of ChocolateBoilerTest
```
Filling the boiler with a chocolate mixture
Boiling the chocolate mixture
Draining the boiled chocolate
Are both boilers the same instance? true
Filling the boiler with a chocolate mixture
```

## When to Use the Singleton Pattern
- When exactly one instance of a class is needed (e.g., logging, configuration, database connections).
- When a single point of access is required for a shared resource.
- Examples: Logger classes, thread pools, or configuration managers.

## Advantages
- Ensures a single instance, reducing resource usage.
- Provides a global access point for consistent access.
- Simplifies resource management for shared objects.

## Disadvantages
- Can introduce global state, making testing and debugging harder.
- May cause issues in multi-threaded environments if not implemented carefully.
- Can violate the Single Responsibility Principle by managing both its instance and business logic.

## Real-World Analogy
Think of a single coffee machine in an office. Only one machine exists (singleton), and everyone accesses it to make coffee, ensuring consistent usage.

## Implementation Notes
- **Eager vs. Lazy Initialization**: This example uses eager initialization. For lazy initialization, initialize `uniqueInstance` in `getInstance()` with synchronization if needed.
- **Thread Safety**: For multi-threaded applications, consider using `synchronized`, double-checked locking, or Java’s `static` holder pattern.
- **Caution**: Overuse of singletons can lead to tightly coupled code and should be used sparingly.