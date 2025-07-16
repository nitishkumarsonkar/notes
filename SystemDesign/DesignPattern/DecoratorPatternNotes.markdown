# Decorator Pattern (Structural)

The **Decorator Pattern** attaches additional responsibilities to an object dynamically without modifying its code. It uses composition to wrap objects, providing a flexible alternative to subclassing for extending functionality.

## Key Components
1. **Component Interface**: Defines the interface for objects that can have responsibilities added (e.g., `Beverage`).
2. **Concrete Component**: The base object to be decorated (e.g., `Espresso`, `HouseBlend`).
3. **Decorator**: An abstract class that implements the component interface and contains a reference to a component (e.g., `CondimentDecorator`).
4. **Concrete Decorators**: Add specific behaviors to the component (e.g., `Mocha`, `Whip`).
5. **Client**: Creates and configures the decorated objects.

## Java Code Example: Coffee Shop

### 1. Component Interface
```java
public abstract class Beverage {
    String description = "Unknown Beverage";

    public String getDescription() {
        return description;
    }

    public abstract double cost();
}
```

### 2. Concrete Components
```java
public class Espresso extends Beverage {
    public Espresso() {
        description = "Espresso";
    }

    public double cost() {
        return 1.99;
    }
}

public class HouseBlend extends Beverage {
    public HouseBlend() {
        description = "House Blend Coffee";
    }

    public double cost() {
        return 0.89;
    }
}
```

### 3. Decorator (Abstract)
```java
public abstract class CondimentDecorator extends Beverage {
    protected Beverage beverage;

    public abstract String getDescription();
}
```

### 4. Concrete Decorators
```java
public class Mocha extends CondimentDecorator {
    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Mocha";
    }

    public double cost() {
        return beverage.cost() + 0.20;
    }
}

public class Whip extends CondimentDecorator {
    public Whip(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Whip";
    }

    public double cost() {
        return beverage.cost() + 0.10;
    }
}

public class Soy extends CondimentDecorator {
    public Soy(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Soy";
    }

    public double cost() {
        return beverage.cost() + 0.15;
    }
}
```

### 5. Client (StarbuzzCoffee)
```java
public class StarbuzzCoffee {
    public static void main(String[] args) {
        // Order an Espresso
        Beverage espresso = new Espresso();
        System.out.println(espresso.getDescription() + " $" + espresso.cost());

        // Order a House Blend with Mocha and Whip
        Beverage houseBlend = new HouseBlend();
        houseBlend = new Mocha(houseBlend);
        houseBlend = new Whip(houseBlend);
        System.out.println(houseBlend.getDescription() + " $" + houseBlend.cost());

        // Order an Espresso with double Mocha, Soy, and Whip
        Beverage espressoFancy = new Espresso();
        espressoFancy = new Mocha(espressoFancy);
        espressoFancy = new Mocha(espressoFancy);
        espressoFancy = new Soy(espressoFancy);
        espressoFancy = new Whip(espressoFancy);
        System.out.println(espressoFancy.getDescription() + " $" + espressoFancy.cost());
    }
}
```

## How It Works
1. **Component and Decorator Relationship**:
   - `Beverage` is the component interface with methods `getDescription` and `cost`.
   - `CondimentDecorator` extends `Beverage` and holds a reference to a `Beverage` object, allowing decorators to wrap components.

2. **Wrapping Mechanism**:
   - Each decorator (e.g., `Mocha`, `Whip`) wraps a `Beverage` object, adding its own behavior (cost and description) before/after delegating to the wrapped object.

3. **Dynamic Extension**:
   - Decorators can be stacked (e.g., `Mocha` on `Whip` on `Espresso`) to combine multiple behaviors dynamically.

4. **Loose Coupling**:
   - Decorators and components share the same interface (`Beverage`), ensuring compatibility and flexibility.

## Output of StarbuzzCoffee
```
Espresso $1.99
House Blend Coffee, Mocha, Whip $1.19
Espresso, Mocha, Mocha, Soy, Whip $2.64
```

## When to Use the Decorator Pattern
- When you need to add responsibilities to objects dynamically and transparently.
- When subclassing would lead to an explosion of subclasses (e.g., every combination of coffee and condiments).
- Examples: Java I/O streams (e.g., `BufferedInputStream` wrapping `FileInputStream`), UI component customization.

## Advantages
- Provides a flexible alternative to subclassing for extending functionality.
- Allows dynamic addition/removal of behaviors at runtime.
- Supports multiple combinations of features without modifying existing code.

## Disadvantages
- Can lead to many small classes, increasing complexity.
- Requires careful management of decorator stacking to avoid errors.
- May be overkill for simple extensions.

## Real-World Analogy
Think of decorating a plain cake (component) with layers of frosting, sprinkles, or toppings (decorators). Each topping adds to the cake without changing its core.