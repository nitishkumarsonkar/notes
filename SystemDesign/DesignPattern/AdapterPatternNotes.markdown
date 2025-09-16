# Adapter Pattern (Structural)

The **Adapter Pattern** converts the interface of a class into another interface that a client expects, enabling incompatible interfaces to work together. It acts as a bridge between two systems, often used for integrating legacy code or third-party libraries.

## Key Components
1. **Target Interface**: The interface the client expects (e.g., `Duck`).
2. **Adaptee**: The existing class with an incompatible interface (e.g., `Turkey`).
3. **Adapter**: Implements the target interface and wraps the adaptee to translate calls (e.g., `TurkeyAdapter`).
4. **Client**: Uses the target interface, unaware of the adaptee.

## Java Code Example: Turkey to Duck Adapter

### 1. Target Interface
```java
public interface Duck {
    void quack();
    void fly();
}
```

### 2. Concrete Target (Duck)
```java
public class MallardDuck implements Duck {
    public void quack() {
        System.out.println("Quack");
    }

    public void fly() {
        System.out.println("I'm flying");
    }
}
```

### 3. Adaptee Interface
```java
public interface Turkey {
    void gobble();
    void fly();
}
```

### 4. Concrete Adaptee (Turkey)
```java
public class WildTurkey implements Turkey {
    public void gobble() {
        System.out.println("Gobble gobble");
    }

    public void fly() {
        System.out.println("I'm flying a short distance");
    }
}
```

### 5. Adapter
```java
public class TurkeyAdapter implements Duck {
    private Turkey turkey;

    public TurkeyAdapter(Turkey turkey) {
        this.turkey = turkey;
    }

    public void quack() {
        turkey.gobble(); // Translate gobble to quack
    }

    public void fly() {
        // Turkeys fly shorter distances, so call fly multiple times to simulate duck flight
        for (int i = 0; i < 5; i++) {
            turkey.fly();
        }
    }
}
```

### 6. Client (DuckTestDrive)
```java
public class DuckTestDrive {
    public static void main(String[] args) {
        // Create a real duck
        Duck duck = new MallardDuck();

        // Create a turkey
        Turkey turkey = new WildTurkey();

        // Create an adapter to make turkey act like a duck
        Duck turkeyAdapter = new TurkeyAdapter(turkey);

        System.out.println("The Duck says...");
        testDuck(duck);

        System.out.println("\nThe Turkey says...");
        turkey.gobble();
        turkey.fly();

        System.out.println("\nThe TurkeyAdapter says...");
        testDuck(turkeyAdapter);
    }

    // Test method that expects a Duck interface
    public static void testDuck(Duck duck) {
        duck.quack();
        duck.fly();
    }
}
```

## How It Works
1. **Target Interface**:
   - The `Duck` interface defines the methods (`quack`, `fly`) that the client expects.

2. **Adaptee**:
   - The `Turkey` interface has incompatible methods (`gobble`, `fly` with different behavior).

3. **Adapter**:
   - The `TurkeyAdapter` implements the `Duck` interface and wraps a `Turkey` object.
   - It translates `quack()` to `gobble()` and adapts `fly()` by calling the turkey’s `fly()` multiple times to mimic a duck’s longer flight.

4. **Client**:
   - The client (`testDuck`) interacts with the `Duck` interface, unaware whether it’s a real `Duck` or a `TurkeyAdapter`.

## Output of DuckTestDrive
```
The Duck says...
Quack
I'm flying

The Turkey says...
Gobble gobble
I'm flying a short distance

The TurkeyAdapter says...
Gobble gobble
I'm flying a short distance
I'm flying a short distance
I'm flying a short distance
I'm flying a short distance
I'm flying a short distance
```

## When to Use the Adapter Pattern
- When you need to integrate a class with an incompatible interface into an existing system.
- When working with legacy systems or third-party libraries that don’t match your interface.
- Examples: Adapting old APIs to new systems, database drivers, or UI component libraries.

## Advantages
- Enables compatibility between incompatible interfaces.
- Promotes reuse of existing classes without modification.
- Supports the Open/Closed Principle by extending functionality via adapters.

## Disadvantages
- Adds complexity with additional classes.
- May require multiple adapters for complex interface mismatches.
- Can introduce performance overhead if adaptation logic is complex.

## Real-World Analogy
Think of a power adapter that converts a European plug (adaptee) to fit a US outlet (target interface). The adapter allows the device to work in a system it wasn’t originally designed for.