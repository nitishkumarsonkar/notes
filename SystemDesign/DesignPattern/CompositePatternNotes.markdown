# Composite Pattern (Structural)

The **Composite Pattern** allows you to treat individual objects and compositions of objects uniformly. It is used to represent part-whole hierarchies, enabling clients to work with both single objects and groups in the same way.

## Key Components
1. **Component Interface**: Defines operations for both leaf and composite nodes (e.g., `MenuComponent`).
2. **Leaf**: Represents individual objects with no children (e.g., `MenuItem`).
3. **Composite**: Represents groups of components, including other composites or leaves (e.g., `Menu`).
4. **Client**: Interacts with the component interface, unaware of whether it’s a leaf or composite.

## Java Code Example: Menu System

### 1. Component Interface
```java
public abstract class MenuComponent {
    public void add(MenuComponent menuComponent) {
        throw new UnsupportedOperationException();
    }

    public void remove(MenuComponent menuComponent) {
        throw new UnsupportedOperationException();
    }

    public MenuComponent getChild(int i) {
        throw new UnsupportedOperationException();
    }

    public String getName() {
        throw new UnsupportedOperationException();
    }

    public double getPrice() {
        throw new UnsupportedOperationException();
    }

    public boolean isVegetarian() {
        throw new UnsupportedOperationException();
    }

    public void print() {
        throw new UnsupportedOperationException();
    }
}
```

### 2. Leaf Class (MenuItem)
```java
public class MenuItem extends MenuComponent {
    private String name;
    private double price;
    private boolean vegetarian;

    public MenuItem(String name, double price, boolean vegetarian) {
        this.name = name;
        this.price = price;
        this.vegetarian = vegetarian;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    public boolean isVegetarian() {
        return vegetarian;
    }

    public void print() {
        System.out.print("  " + getName());
        if (isVegetarian()) {
            System.out.print("(v)");
        }
        System.out.println(", $" + getPrice());
    }
}
```

### 3. Composite Class (Menu)
```java
import java.util.ArrayList;
import java.util.List;

public class Menu extends MenuComponent {
    private List<MenuComponent> menuComponents = new ArrayList<>();
    private String name;

    public Menu(String name) {
        this.name = name;
    }

    public void add(MenuComponent menuComponent) {
        menuComponents.add(menuComponent);
    }

    public void remove(MenuComponent menuComponent) {
        menuComponents.remove(menuComponent);
    }

    public MenuComponent getChild(int i) {
        return menuComponents.get(i);
    }

    public String getName() {
        return name;
    }

    public void print() {
        System.out.println("\n" + getName() + " Menu");
        System.out.println("---------------------");
        for (MenuComponent component : menuComponents) {
            component.print();
        }
    }
}
```

### 4. Client (Waitress)
```java
public class Waitress {
    private MenuComponent allMenus;

    public Waitress(MenuComponent allMenus) {
        this.allMenus = allMenus;
    }

    public void printMenu() {
        allMenus.print();
    }
}
```

### 5. Client (MenuTestDrive)
```java
public class MenuTestDrive {
    public static void main(String[] args) {
        // Create menu hierarchy
        MenuComponent pancakeHouseMenu = new Menu("Pancake House");
        MenuComponent dinerMenu = new Menu("Diner");
        MenuComponent dessertMenu = new Menu("Dessert");

        MenuComponent allMenus = new Menu("All Menus");

        // Build the hierarchy
        allMenus.add(pancakeHouseMenu);
        allMenus.add(dinerMenu);
        dinerMenu.add(dessertMenu);

        // Add menu items
        pancakeHouseMenu.add(new MenuItem("Pancake Breakfast", 2.99, true));
        pancakeHouseMenu.add(new MenuItem("Waffle Special", 3.49, false));
        dinerMenu.add(new MenuItem("Vegetarian BLT", 2.99, true));
        dessertMenu.add(new MenuItem("Apple Pie", 1.59, true));

        // Create waitress and print menu
        Waitress waitress = new Waitress(allMenus);
        waitress.printMenu();
    }
}
```

## How It Works
1. **Component Interface**:
   - `MenuComponent` defines common operations for both leaves (`MenuItem`) and composites (`Menu`).

2. **Leaf**:
   - `MenuItem` represents individual menu items with properties like name, price, and vegetarian status.

3. **Composite**:
   - `Menu` represents a group of `MenuComponent` objects (items or sub-menus), managing a list of children.

4. **Uniform Treatment**:
   - The `print` method is implemented recursively, allowing clients to treat leaves and composites the same way.

5. **Client**:
   - The `Waitress` uses the `MenuComponent` interface to print the entire menu hierarchy without knowing its structure.

## Output of MenuTestDrive
```
All Menus Menu
---------------------

Pancake House Menu
---------------------
  Pancake Breakfast(v), $2.99
  Waffle Special, $3.49

Diner Menu
---------------------
  Vegetarian BLT(v), $2.99

Dessert Menu
---------------------
  Apple Pie(v), $1.59
```

## When to Use the Composite Pattern
- When you need to represent part-whole hierarchies (e.g., file systems, UI components).
- When clients should treat individual objects and compositions uniformly.
- Examples: GUI frameworks (e.g., panels containing buttons or other panels), organizational charts.

## Advantages
- Simplifies client code by treating leaves and composites uniformly.
- Makes it easy to add new components (leaves or composites).
- Supports recursive structures naturally.

## Disadvantages
- Can make the design overly general, as all components share the same interface.
- May lead to runtime errors if operations are unsupported by certain components (e.g., `add` on a leaf).
- Can be complex to manage large hierarchies.

## Real-World Analogy
Think of a file system where files (leaves) and folders (composites) are treated as file system objects. You can perform operations like listing contents on both, but folders may contain other files or folders.