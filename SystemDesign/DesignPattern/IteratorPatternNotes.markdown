# Iterator Pattern (Behavioral)

The **Iterator Pattern** provides a way to access elements of a collection sequentially without exposing its underlying structure. It encapsulates traversal logic, allowing clients to iterate over different data structures uniformly.

## Key Components
1. **Iterator Interface**: Defines methods for traversing a collection (e.g., `hasNext`, `next`).
2. **Concrete Iterator**: Implements the iterator interface for a specific collection (e.g., `DinerMenuIterator`).
3. **Aggregate Interface**: Defines a method to create an iterator (e.g., `Menu`).
4. **Concrete Aggregate**: Implements the aggregate interface and holds the collection (e.g., `DinerMenu`).
5. **Client**: Uses the iterator to traverse the collection.

## Java Code Example: Diner Menu

### 1. Iterator Interface
```java
public interface Iterator {
    boolean hasNext();
    Object next();
}
```

### 2. Aggregate Interface
```java
public interface Menu {
    Iterator createIterator();
}
```

### 3. Concrete Aggregate (DinerMenu)
```java
public class DinerMenu implements Menu {
    private MenuItem[] items;
    private int numberOfItems = 0;
    private static final int MAX_ITEMS = 6;

    public DinerMenu() {
        items = new MenuItem[MAX_ITEMS];
        addItem("Vegetarian BLT", "Bacon with lettuce & tomato on whole wheat", true, 2.99);
        addItem("BLT", "Bacon with lettuce & tomato on white bread", false, 2.99);
        addItem("Soup of the day", "Soup with a side of potato salad", false, 3.29);
    }

    public void addItem(String name, String description, boolean vegetarian, double price) {
        if (numberOfItems >= MAX_ITEMS) {
            System.out.println("Menu is full! Can't add item.");
        } else {
            items[numberOfItems] = new MenuItem(name, description, vegetarian, price);
            numberOfItems++;
        }
    }

    public Iterator createIterator() {
        return new DinerMenuIterator(items);
    }
}
```

### 4. Concrete Iterator (DinerMenuIterator)
```java
public class DinerMenuIterator implements Iterator {
    private MenuItem[] items;
    private int position = 0;

    public DinerMenuIterator(MenuItem[] items) {
        this.items = items;
    }

    public boolean hasNext() {
        if (position >= items.length || items[position] == null) {
            return false;
        }
        return true;
    }

    public Object next() {
        MenuItem menuItem = items[position];
        position++;
        return menuItem;
    }
}
```

### 5. Helper Class (MenuItem)
```java
public class MenuItem {
    private String name;
    private String description;
    private boolean vegetarian;
    private double price;

    public MenuItem(String name, String description, boolean vegetarian, double price) {
        this.name = name;
        this.description = description;
        this.vegetarian = vegetarian;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }

    public boolean isVegetarian() {
        return vegetarian;
    }

    public double getPrice() {
        return price;
    }

    public String toString() {
        return name + ", $" + price + "\n   " + description;
    }
}
```

### 6. Client (Waitress)
```java
public class Waitress {
    private Menu dinerMenu;

    public Waitress(Menu dinerMenu) {
        this.dinerMenu = dinerMenu;
    }

    public void printMenu() {
        Iterator iterator = dinerMenu.createIterator();
        System.out.println("MENU\n----\nDINER MENU");
        printMenu(iterator);
    }

    private void printMenu(Iterator iterator) {
        while (iterator.hasNext()) {
            MenuItem menuItem = (MenuItem) iterator.next();
            System.out.println(menuItem);
        }
    }
}
```

### 7. Client (MenuTestDrive)
```java
public class MenuTestDrive {
    public static void main(String[] args) {
        // Create a diner menu
        DinerMenu dinerMenu = new DinerMenu();

        // Create a waitress to print the menu
        Waitress waitress = new Waitress(dinerMenu);
        waitress.printMenu();
    }
}
```

## How It Works
1. **Iterator Interface**:
   - Defines `hasNext` and `next` for traversing the collection.

2. **Concrete Iterator**:
   - `DinerMenuIterator` implements traversal for an array-based menu, tracking position and checking boundaries.

3. **Aggregate**:
   - `DinerMenu` implements the `Menu` interface, providing a `createIterator` method to return an iterator.

4. **Client**:
   - The `Waitress` uses the iterator to traverse the menu without knowing its internal structure (e.g., array).

5. **Encapsulation**:
   - The iterator hides the collection’s implementation, allowing the client to focus on traversal logic.

## Output of MenuTestDrive
```
MENU
----
DINER MENU
Vegetarian BLT, $2.99
   Bacon with lettuce & tomato on whole wheat
BLT, $2.99
   Bacon with lettuce & tomato on white bread
Soup of the day, $3.29
   Soup with a side of potato salad
```

## When to Use the Iterator Pattern
- When you need to traverse different data structures (e.g., arrays, lists, trees) uniformly.
- When you want to hide the internal structure of a collection from clients.
- Examples: Java’s `Iterator` interface, database cursors, or file system traversal.

## Advantages
- Encapsulates traversal logic, simplifying client code.
- Supports multiple collection types with a uniform interface.
- Allows adding new collection types without changing client code.

## Disadvantages
- May be overkill for simple collections with direct access.
- Adds complexity with additional iterator classes.
- May not support advanced operations like removing elements during traversal (unless explicitly implemented).

## Real-World Analogy
Think of a TV remote (iterator) that lets you cycle through channels (elements) without needing to know how the TV stores or manages the channel list.