# Strategy Pattern (Behavioral)

The **Strategy Pattern** is a behavioral design pattern that defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the client to choose an algorithm at runtime without modifying the context class. This pattern follows the principle of encapsulating what varies and programming to an interface.

## Key Components

1. **Strategy Interface**: Defines the contract for the algorithm (e.g., `FlyBehavior`, `QuackBehavior`).
2. **Concrete Strategies**: Implement the strategy interface with specific behaviors (e.g., `FlyWithWings`, `FlyNoWay`, `Quack`, `Squeak`).
3. **Context**: The class that uses the strategy (e.g., `Duck`).
4. **Client**: Sets the strategy at runtime (e.g., `Main` class).

## Java Code Example: Duck Simulation

### 1. Strategy Interfaces

```java
// Interface for flying behavior
public interface FlyBehavior {
    void fly();
}

// Interface for quacking behavior
public interface QuackBehavior {
    void quack();
}
```

### 2. Concrete Strategy Classes

```java
// Flying behaviors
public class FlyWithWings implements FlyBehavior {
    public void fly() {
        System.out.println("I'm flying with wings!");
    }
}

public class FlyNoWay implements FlyBehavior {
    public void fly() {
        System.out.println("I can't fly!");
    }
}

// Quacking behaviors
public class Quack implements QuackBehavior {
    public void quack() {
        System.out.println("Quack! Quack!");
    }
}

public class Squeak implements QuackBehavior {
    public void quack() {
        System.out.println("Squeak!");
    }
}

public class MuteQuack implements QuackBehavior {
    public void quack() {
        System.out.println("<< Silence >>");
    }
}
```

### 3. Context (Duck Class)

```java
public abstract class Duck {
    // Reference to strategy interfaces
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    // Constructor (can be overridden or set via setters)
    public Duck(FlyBehavior flyBehavior, QuackBehavior quackBehavior) {
        this.flyBehavior = flyBehavior;
        this.quackBehavior = quackBehavior;
    }

    // Delegate behavior to strategy objects
    public void performFly() {
        flyBehavior.fly();
    }

    public void performQuack() {
        quackBehavior.quack();
    }

    // Allow dynamic behavior change
    public void setFlyBehavior(FlyBehavior fb) {
        this.flyBehavior = fb;
    }

    public void setQuackBehavior(QuackBehavior qb) {
        this.quackBehavior = qb;
    }

    // Common behavior
    public void swim() {
        System.out.println("All ducks float, even decoys!");
    }

    public abstract void display(); // Abstract method for specific duck appearance
}
```

### 4. Concrete Duck Classes

```java
public class MallardDuck extends Duck {
    public MallardDuck() {
        super(new FlyWithWings(), new Quack());
    }

    public void display() {
        System.out.println("I'm a real Mallard duck!");
    }
}

public class RubberDuck extends Duck {
    public RubberDuck() {
        super(new FlyNoWay(), new Squeak());
    }

   

 public void display() {
        System.out.println("I'm a rubber duck!");
    }
}

public class DecoyDuck extends Duck {
    public DecoyDuck() {
        super(new FlyNoWay(), new MuteQuack());
    }

    public void display() {
        System.out.println("I'm a decoy duck!");
    }
}
```

### 5. Client (Main Class)

```java
public class DuckSimulator {
    public static void main(String[] args) {
        // Create a MallardDuck with default behaviors
        Duck mallard = new MallardDuck();
        mallard.display();
        mallard.performFly();  // Output: I'm flying with wings!
        mallard.performQuack(); // Output: Quack! Quack!
        mallard.swim();       // Output: All ducks float, even decoys!

        System.out.println("---");

        // Create a RubberDuck
        Duck rubber = new RubberDuck();
        rubber.display();
        rubber.performFly();  // Output: I can't fly!
        rubber.performQuack(); // Output: Squeak!
        rubber.swim();        // Output: All ducks float, even decoys!

        System.out.println("---");

        // Dynamically change behavior of MallardDuck
        mallard.setFlyBehavior(new FlyNoWay());
        mallard.setQuackBehavior(new MuteQuack());
        mallard.display();
        mallard.performFly();  // Output: I can't fly!
        mallard.performQuack(); // Output: << Silence >>
    }
}
```

## How It Works

1. **Encapsulation of Behaviors**:

   - `FlyBehavior` and `QuackBehavior` interfaces encapsulate flying and quacking algorithms.
   - Concrete classes like `FlyWithWings` or `Squeak` provide specific implementations.

2. **Composition Over Inheritance**:

   - Instead of hardcoding behaviors in subclasses, `Duck` uses composition to hold references to `FlyBehavior` and `QuackBehavior`.
   - This makes behaviors interchangeable without modifying the `Duck` class.

3. **Dynamic Behavior Change**:

   - The `setFlyBehavior` and `setQuackBehavior` methods allow runtime behavior changes (e.g., making a `MallardDuck` sto p flying).

4. **Loose Coupling**:

   - The `Duck` class depends on abstractions (`FlyBehavior`, `QuackBehavior`), not concrete implementations, reducing dependencies.

## Output of DuckSimulator

```
I'm a real Mallard duck!
I'm flying with wings!
Quack! Quack!
All ducks float, even decoys!
---
I'm a rubber duck!
I can't fly!
Squeak!
All ducks float, even decoys!
---
I'm a real Mallard duck!
I can't fly!
<< Silence >>
```

## When to Use the Strategy Pattern

- When you have multiple algorithms or behaviors that should be interchangeable.
- When you want to avoid conditional statements (e.g., `if (type == "flying")`).
- When behaviors need to be changed dynamically at runtime.
- Examples: Sorting algorithms, payment methods, or game character abilities.

## Advantages

- Promotes flexibility and reusability.
- Reduces tight coupling between classes.
- Simplifies adding new behaviors (just create a new strategy class).
- Avoids code duplication and conditionals.

## Disadvantages

- Increases the number of classes (one per strategy).
- Clients must be aware of available strategies to choose them.
- Adds complexity for simple scenarios.

## Real-World Analogy

Think of a smartphone with interchangeable camera modes (e.g., portrait, night, panoramic). The phone (context) delegates to the selected mode (strategy) without changing its core functionality.