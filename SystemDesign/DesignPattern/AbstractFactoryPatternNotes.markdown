# Abstract Factory Pattern (Creational)

The **Abstract Factory Pattern** provides an interface for creating families of related or dependent objects without specifying their concrete classes. It ensures consistency among products in a family, making it ideal for systems requiring multiple related objects.
## Key Components
1. **Abstract Factory Interface**: Defines methods for creating products in a family (e.g., `PizzaIngredientFactory`).
2. **Concrete Factories**: Implement the factory interface to create specific product families (e.g., `NYPizzaIngredientFactory`, `ChicagoPizzaIngredientFactory`).
3. **Abstract Products**: Interfaces for each type of product (e.g., `Dough`, `Sauce`).
4. **Concrete Products**: Implementations of the product interfaces (e.g., `ThinCrustDough`, `MarinaraSauce`).
5. **Client**: Uses the abstract factory to create products without knowing their concrete classes.

## Java Code Example: Pizza Ingredient Factory

### 1. Abstract Product Interfaces
```java
public interface Dough {
    String toString();
}

public interface Sauce {
    String toString();
}

public interface Cheese {
    String toString();
}
```

### 2. Concrete Products
```java
public class ThinCrustDough implements Dough {
    public String toString() {
        return "Thin Crust Dough";
    }
}

public class ThickCrustDough implements Dough {
    public String toString() {
        return "Thick Crust Dough";
    }
}

public class MarinaraSauce implements Sauce {
    public String toString() {
        return "Marinara Sauce";
    }
}

public class PlumTomatoSauce implements Sauce {
    public String toString() {
        return "Plum Tomato Sauce";
    }
}

public class MozzarellaCheese implements Cheese {
    public String toString() {
        return "Mozzarella Cheese";
    }
}

public class ReggianoCheese implements Cheese {
    public String toString() {
        return "Reggiano Cheese";
    }
}
```

### 3. Abstract Factory Interface
```java
public interface PizzaIngredientFactory {
    Dough createDough();
    Sauce createSauce();
    Cheese createCheese();
}
```

### 4. Concrete Factories
```java
public class NYPizzaIngredientFactory implements PizzaIngredientFactory {
    public Dough createDough() {
        return new ThinCrustDough();
    }

    public Sauce createSauce() {
        return new MarinaraSauce();
    }

    public Cheese createCheese() {
        return new ReggianoCheese();
    }
}

public class ChicagoPizzaIngredientFactory implements PizzaIngredientFactory {
    public Dough createDough() {
        return new ThickCrustDough();
    }

    public Sauce createSauce() {
        return new PlumTomatoSauce();
    }

    public Cheese createCheese() {
        return new MozzarellaCheese();
    }
}
```

### 5. Product (Pizza)
```java
public abstract class Pizza {
    String name;
    Dough dough;
    Sauce sauce;
    Cheese cheese;

    abstract void prepare();

    void bake() {
        System.out.println("Baking " + name);
    }

    void cut() {
        System.out.println("Cutting " + name);
    }

    void box() {
        System.out.println("Boxing " + name);
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

public class CheesePizza extends Pizza {
    PizzaIngredientFactory ingredientFactory;

    public CheesePizza(PizzaIngredientFactory ingredientFactory) {
        this.ingredientFactory = ingredientFactory;
    }

    void prepare() {
        System.out.println("Preparing " + name);
        dough = ingredientFactory.createDough();
        sauce = ingredientFactory.createSauce();
        cheese = ingredientFactory.createCheese();
        System.out.println("Adding " + dough + ", " + sauce + ", " + cheese);
    }
}
```

### 6. Client (PizzaStore and PizzaTestDrive)
```java
public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
        Pizza pizza = createPizza(type);
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }

    protected abstract Pizza createPizza(String type);
}

public class NYPizzaStore extends PizzaStore {
    protected Pizza createPizza(String type) {
        Pizza pizza = null;
        PizzaIngredientFactory SOS
ingredientFactory = new NYPizzaIngredientFactory();

        if (type.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("New York Style Cheese Pizza");
        }
        return pizza;
    }
}

public class ChicagoPizzaStore extends PizzaStore {
    protected Pizza createPizza(String type) {
        Pizza pizza = null;
        PizzaIngredientFactory ingredientFactory = new ChicagoPizzaIngredientFactory();

        if (type.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("Chicago Style Cheese Pizza");
        }
        return pizza;
    }
}

public class PizzaTestDrive {
    public static void main(String[] args) {
        // Create NY and Chicago stores
        PizzaStore nyStore = new NYPizzaStore();
        PizzaStore chicagoStore = new ChicagoPizzaStore();

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
1. **Abstract Factory**:
   - `PizzaIngredientFactory` defines methods to create a family of ingredients (`Dough`, `Sauce`, `Cheese`).
   - Concrete factories (`NYPizzaIngredientFactory`, `ChicagoPizzaIngredientFactory`) produce region-specific ingredients.

2. **Family of Products**:
   - Each factory creates a consistent set of products (e.g., NY factory creates `ThinCrustDough`, `MarinaraSauce`, `ReggianoCheese`).

3. **Client Usage**:
   - The `PizzaStore` uses the factory to create ingredients for a `Pizza`, without knowing the concrete classes.

4. **Encapsulation**:
   - Object creation is encapsulated in the factory, ensuring the client interacts with interfaces, not implementations.

## Output of PizzaTestDrive
```
Preparing New York Style Cheese Pizza
Adding Thin Crust Dough, Marinara Sauce, Reggiano Cheese
Baking New York Style Cheese Pizza
Cutting New York Style Cheese Pizza
Boxing New York Style Cheese Pizza
Ordered a New York Style Cheese Pizza

Preparing Chicago Style Cheese Pizza
Adding Thick Crust Dough, Plum Tomato Sauce, Mozzarella Cheese
Baking Chicago Style Cheese Pizza
Cutting Chicago Style Cheese Pizza
Boxing Chicago Style Cheese Pizza
Ordered a Chicago Style Cheese Pizza
```

## When to Use the Abstract Factory Pattern
- When you need to create families of related objects with consistent interfaces.
- When you want to ensure products work together correctly (e.g., NY-style ingredients).
- Examples: UI toolkits (e.g., Windows vs. Mac widgets), database drivers, or game asset factories.

## Advantages
- Ensures consistency among products in a family.
- Promotes loose coupling by isolating client code from concrete classes.
- Simplifies adding new product families via new factory classes.

## Disadvantages
- Increases complexity with additional interfaces and classes.
- Can be overkill for simple systems with few product families.
- Modifying the factory interface requires updating all concrete factories.

## Real-World Analogy
Think of a car manufacturer with factories for different regions (e.g., US, Europe). Each factory produces a family of parts (engine, chassis, interior) tailored to the region, ensuring compatibility.
