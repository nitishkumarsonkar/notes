# Template Method Pattern (Behavioral)

The **Template Method Pattern** defines the skeleton of an algorithm in a method, deferring some steps to subclasses. It allows subclasses to customize specific steps without altering the algorithm's overall structure, promoting code reuse and consistency.

## Key Components
1. **Abstract Class**: Defines the template method and abstract steps to be implemented by subclasses (e.g., `CaffeineBeverage`).
2. **Template Method**: The method that outlines the algorithm, calling both fixed and customizable steps.
3. **Concrete Classes**: Implement the abstract steps to customize the algorithm (e.g., `Tea`, `Coffee`).
4. **Client**: Uses the concrete classes, invoking the template method.

## Java Code Example: Caffeine Beverage

### 1. Abstract Class (CaffeineBeverage)
```java
public abstract class CaffeineBeverage {
    // Template method defining the algorithm
    final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    // Abstract methods to be implemented by subclasses
    abstract void brew();
    abstract void addCondiments();

    // Concrete methods (fixed steps)
    void boilWater() {
        System.out.println("Boiling water");
    }

    void pourInCup() {
        System.out.println("Pouring into cup");
    }
}
```

### 2. Concrete Classes
```java
public class Tea extends CaffeineBeverage {
    void brew() {
        System.out.println("Steeping the tea");
    }

    void addCondiments() {
        System.out.println("Adding lemon");
    }
}

public class Coffee extends CaffeineBeverage {
    void brew() {
        System.out.println("Dripping coffee through filter");
    }

    void addCondiments() {
        System.out.println("Adding sugar and milk");
    }
}
```

### 3. Client (BeverageTestDrive)
```java
public class BeverageTestDrive {
    public static void main(String[] args) {
        // Create and prepare a tea
        CaffeineBeverage tea = new Tea();
        System.out.println("Making tea...");
        tea.prepareRecipe();

        System.out.println();

        // Create and prepare a coffee
        CaffeineBeverage coffee = new Coffee();
        System.out.println("Making coffee...");
        coffee.prepareRecipe();
    }
}
```

## How It Works
1. **Template Method**:
   - The `prepareRecipe` method in `CaffeineBeverage` defines the algorithm's structure, calling fixed steps (`boilWater`, `pourInCup`) and abstract steps (`brew`, `addCondiments`).

2. **Customization**:
   - Subclasses (`Tea`, `Coffee`) implement the abstract methods to provide specific behavior for brewing and adding condiments.

3. **Fixed Structure**:
   - The `final` keyword on `prepareRecipe` ensures subclasses cannot alter the algorithm's structure, only the customizable steps.

4. **Code Reuse**:
   - Common steps (`boilWater`, `pourInCup`) are defined once in the abstract class, reducing duplication.

## Output of BeverageTestDrive
```
Making tea...
Boiling water
Steeping the tea
Pouring into cup
Adding lemon

Making coffee...
Boiling water
Dripping coffee through filter
Pouring into cup
Adding sugar and milk
```

## When to Use the Template Method Pattern
- When you have a fixed algorithm with some variable steps.
- When you want to avoid code duplication by centralizing common behavior.
- Examples: Frameworks (e.g., Java’s `AbstractList`), data processing pipelines, or UI rendering processes.

## Advantages
- Promotes code reuse by centralizing the algorithm’s structure.
- Ensures consistency in the algorithm across subclasses.
- Allows customization of specific steps without changing the overall flow.

## Disadvantages
- Can limit flexibility if the algorithm’s structure needs significant changes.
- Relies on inheritance, which may lead to rigid class hierarchies.
- Subclasses may need to implement unnecessary methods.

## Real-World Analogy
Think of a recipe for baking a cake (template method). The steps (mix ingredients, bake, cool) are fixed, but the type of cake (chocolate, vanilla) customizes specific steps like adding flavorings.