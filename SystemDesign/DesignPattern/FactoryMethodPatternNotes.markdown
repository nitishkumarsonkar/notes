# Factory Method Pattern (Creational)

The **Factory Method Pattern** defines an interface for creating objects, letting subclasses decide which class to instantiate. It promotes loose coupling by deferring instantiation to subclasses, allowing flexibility in object creation.

## Key Components
1. **Product Interface**: Defines the interface for objects the factory creates (e.g., `Pizza`).
2. **Concrete Products**: Implement the product interface (e.g., `NYStyleCheesePizza`, `ChicagoStyleCheesePizza`).
3. **Creator**: Declares the factory method that returns a product (e.g., `PizzaStore`).
4. **Concrete Creators**: Override the factory method to create specific products (e.g., `NYStylePizzaStore`, `ChicagoStylePizzaStore`).
5. **Client**: Uses the creator to create products without knowing their concrete classes.

## Java Code Example: Pizza Store

### 1. Product Interface
```java
public abstract class Pizza {
    String name;

    public void prepare() {
        System.out.println("Preparing " + name);
    }

    public void bake() {
        System.out.println("Baking " + name);
    }

    public void cut() {
        System.out.println("Cutting " + name);
    }

    public void box() {
        System.out.println("Boxing " + name);
    }

    public String getName() {
        return name;
    }
}
```

### 2. Concrete Products
```java
public class NYStyleCheesePizza extends Pizza {
    public NYStyleCheesePizza() {
        name = "NY Style Cheese Pizza";
    }
}

public class ChicagoStyleCheesePizza extends Pizza {
    public ChicagoStyleCheesePizza() {
        name = "Chicago Style Cheese Pizza";
    }

    // Override cut for Chicago-style (deep dish)
    public void cut() {
        System.out.println("Cutting " + name + " into square slices");
    }
}
```

### 3. Creator (Abstract)
```java
public abstract class PizzaStore {
    // Factory method
    protected abstract Pizza createPizza(String type);

    // Order process using the factory method
    public Pizza orderPizza(String type) {
        Pizza pizza = createPizza(type);
        System.out.println("--- Making a " + pizza.getName() + " ---");
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```

### 4. Concrete Creators
```java
public class NYStylePizzaStore extends PizzaStore {
    protected Pizza createPizza(String type) {
        if (type.equals("cheese")) {
            return new NYStyleCheesePizza();
        }
        // Add more pizza types as needed
        return null;
    }
}

public class ChicagoStylePizzaStore extends PizzaStore {
    protected Pizza createPizza(String type) {
        if (type.equals("cheese")) {
            return new ChicagoStyleCheesePizza();
        }
        // Add more pizza types as needed
        return null;
    }
}
```

### 5. Client (PizzaTestDrive)
```java
public class PizzaTestDrive {
    public static void main(String[] args) {
        // Create NY and Chicago stores
        PizzaStore nyStore = new NYStylePizzaStore();
        PizzaStore chicagoStore = new ChicagoStylePizzaStore();

        // Order a cheese pizza from NY store
        Pizza nyPizza = nyStore.orderPizza("cheese");
        System.out.println("Ordered a " + nyPizza.getName() + "\n");

        // Order a cheese pizza from Chicago store
        Pizza chicagoPizza = chicagoStore.orderPizza("cheese");
        System.out.println("Ordered a " + chicagoPizza.getName());
    }
}
```

## How It Works
1. **Factory Method**:
   - The `createPizza` method in `PizzaStore` is the factory method, declared abstract to let subclasses implement it.
   - Subclasses (`NYStylePizzaStore`, `ChicagoStylePizzaStore`) decide which concrete `Pizza` to instantiate.

2. **Encapsulation of Object Creation**:
   - The `orderPizza` method handles the creation and preparation process, delegating object creation to the factory method.

3. **Polymorphism**:
   - The client interacts with the `PizzaStore` and `Pizza` interfaces, unaware of the concrete classes being instantiated.

4. **Extensibility**:
   - New pizza types or stores can be added by creating new subclasses without modifying existing code.

## Output of PizzaTestDrive
```
--- Making a NY Style Cheese Pizza ---
Preparing NY Style Cheese Pizza
Baking NY Style Cheese Pizza
Cutting NY Style Cheese Pizza
Boxing NY Style Cheese Pizza
Ordered a NY Style Cheese Pizza

--- Making a Chicago Style Cheese Pizza ---
Preparing Chicago Style Cheese Pizza
Baking Chicago Style Cheese Pizza
Cutting Chicago Style Cheese Pizza into square slices
Boxing Chicago Style Cheese Pizza
Ordered a Chicago Style Cheese Pizza
```

## When to Use the Factory Method Pattern
- When the exact type of object to create depends on context or configuration.
- When you want to delegate object creation to subclasses.
- Examples: Creating platform-specific UI components, document types in an editor, or game characters.

## Advantages
- Promotes loose coupling by isolating object creation from usage.
- Supports the Open/Closed Principle (OCP) by allowing new products without modifying existing code.
- Simplifies adding new product types via new subclasses.

## Disadvantages
- Increases the number of classes (one creator per product family).
- May add complexity for simple creation scenarios.
- Requires subclasses to implement the factory method.

## Real-World Analogy
Think of a pizza franchise (creator) with a standard process for making pizzas (`orderPizza`). Each regional store (concrete creator) decides how to make specific pizzas (products) like NY or Chicago style.