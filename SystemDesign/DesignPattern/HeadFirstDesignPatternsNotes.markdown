# Head First Design Patterns Notes

*Head First Design Patterns* by Eric Freeman and Elisabeth Robson is a popular book that explains design patterns in an accessible, engaging way. Below are concise notes covering key concepts and patterns from the book, focusing on core ideas, principles, and examples. These notes assume familiarity with object-oriented programming (OOP) concepts like classes, inheritance, and interfaces.

## Key Principles of Design Patterns
1. **Favor composition over inheritance**: Use object composition (HAS-A relationships) to achieve flexibility and reusability instead of rigid inheritance hierarchies.
2. **Program to an interface, not an implementation**: Code to abstractions (interfaces or abstract classes) to reduce dependencies on specific implementations.
3. **Encapsulate what varies**: Isolate parts of the code that change to make the system more flexible and maintainable.
4. **Open/Closed Principle (OCP)**: Classes should be open for extension but closed for modification.
5. **Single Responsibility Principle (SRP)**: A class should have only one reason to change.
6. **Dependency Inversion Principle (DIP)**: High-level modules should not depend on low-level modules; both should depend on abstractions.
7. **Liskov Substitution Principle (LSP)**: Subtypes must be substitutable for their base types without altering program correctness.
8. **Strive for loosely coupled designs**: Reduce dependencies between objects to improve flexibility and maintainability.
9. **Design patterns are tools, not rules**: Apply patterns when they solve a specific problem, not just because they exist.

## Core Design Patterns Covered in the Book
The book covers 12 design patterns, organized by purpose (Creational, Structural, Behavioral). Below is a summary of each pattern, including its intent, structure, and a practical example.

### 1. Strategy Pattern (Behavioral)
- **Intent**: Define a family of algorithms, encapsulate each one, and make them interchangeable. Allows the algorithm to vary independently from the client using it.
- **Structure**: A context class uses a strategy interface, with concrete strategy classes implementing the interface.
- **Example**: A `Duck` class with interchangeable `FlyBehavior` (e.g., `FlyWithWings`, `FlyNoWay`) and `QuackBehavior` (e.g., `Quack`, `Squeak`, `MuteQuack`).
- **Key Idea**: Encapsulate behavior to make it swappable without changing the context.
- **When to Use**: When you need multiple behaviors for an object and want to switch them dynamically.

### 2. Observer Pattern (Behavioral)
- **Intent**: Define a one-to-many dependency where multiple objects (observers) are notified of state changes in a subject.
- **Structure**: A subject maintains a list of observers (via an interface) and notifies them of changes.
- **Example**: A `WeatherStation` (subject) notifies `Display` objects (observers) like `CurrentConditionsDisplay` when weather data changes.
- **Key Idea**: Loose coupling between subject and observers; observers can be added/removed dynamically.
- **When to Use**: When multiple objects need to react to changes in another object (e.g., UI updates, event handling).

### 3. Decorator Pattern (Structural)
- **Intent**: Attach additional responsibilities to an object dynamically without modifying its code.
- **Structure**: A decorator implements the same interface as the component it wraps, adding behavior before/after delegating to the component.
- **Example**: A `Beverage` class (e.g., `Espresso`) wrapped by decorators like `Mocha`, `Whip`, or `Soy` to add costs and descriptions.
- **Key Idea**: Provides a flexible alternative to subclassing for extending functionality.
- **When to Use**: When you need to add features to objects in a flexible, reusable way (e.g., Java I/O streams).

### 4. Factory Method Pattern (Creational)
- **Intent**: Define an interface for creating objects, letting subclasses decide which class to instantiate.
- **Structure**: A creator class declares a factory method, with concrete creators implementing it to return specific products.
- **Example**: A `PizzaStore` with a `createPizza` method overridden by `NYStylePizzaStore` and `ChicagoStylePizzaStore` to create style-specific pizzas.
- **Key Idea**: Defer instantiation to subclasses for flexibility.
- **When to Use**: When the exact type of object to create depends on context or configuration.

### 5. Abstract Factory Pattern (Creational)
- **Intent**: Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
- **Structure**: An abstract factory interface declares methods for creating products; concrete factories implement these for specific families.
- **Example**: A `PizzaIngredientFactory` creates ingredients (e.g., `NYIngredientFactory` for NY-style dough, sauce, etc., vs. `ChicagoIngredientFactory`).
- **Key Idea**: Ensures consistency among products in a family.
- **When to Use**: When you need to create groups of related objects (e.g., UI toolkits for different platforms).

### 6. Singleton Pattern (Creational)
- **Intent**: Ensure a class has only one instance and provide a global point of access to it.
- **Structure**: A class with a private constructor and a static method to access the single instance (often lazily initialized).
- **Example**: A `ChocolateBoiler` class that ensures only one boiler exists to manage chocolate production.
- **Key Idea**: Control access to a single instance, often for resource management.
- **When to Use**: When exactly one instance is needed (e.g., logging, configuration managers). **Caution**: Can introduce global state issues.

### 7. Command Pattern (Behavioral)
- **Intent**: Encapsulate a request as an object, allowing parameterization, queuing, or logging of Casteism
requests.
- **Structure**: A command interface with an `execute` method; concrete commands encapsulate actions and receivers.
- **Example**: A `RemoteControl` with commands like `LightOnCommand` and `LightOffCommand` that invoke actions on a `Light` object.
- **Key Idea**: Decouples the requester of an action from the object performing it.
- **When to Use**: For undo/redo, queuing tasks, or logging operations (e.g., GUI button actions).

### 8. Adapter Pattern (Structural)
- **Intent**: Convert the interface of a class into another interface that a client expects.
- **Structure**: An adapter implements the target interface and wraps an adaptee to translate calls.
- **Example**: A `TurkeyAdapter` makes a `Turkey` object behave like a `Duck` by implementing the `Duck` interface.
- **Key Idea**: Allows incompatible interfaces to work together.
- **When to Use**: When integrating legacy systems or third-party libraries with different interfaces.

### 9. Facade Pattern (Structural)
- **Intent**: Provide a simplified interface to a complex subsystem.
- **Structure**: A facade class offers high-level methods that internally coordinate multiple subsystem classes.
- **Example**: A `HomeTheaterFacade` simplifies operations like `watchMovie` by coordinating a `DVDPlayer`, `Projector`, and `SoundSystem`.
- **Key Idea**: Reduces complexity for clients by hiding subsystem details.
- **When to Use**: When you want to simplify interactions with a complex system (e.g., APIs, libraries).

### 10. Template Method Pattern (Behavioral)
- **Intent**: Define the skeleton of an algorithm in a method, deferring some steps to subclasses.
- **Structure**: An abstract class defines a template method with steps, some of which are implemented by subclasses.
- **Example**: A `CaffeineBeverage` class with a `prepareRecipe` method that calls steps like `boilWater` (fixed) and `addCondiments` (customized by `TeaMatches
` or `Coffee`).
- **Key Idea**: Standardizes the algorithm structure while allowing customization.
- **When to Use**: When you have a fixed process with variable steps (e.g., frameworks, algorithms).

### 11. Iterator Pattern (Behavioral)
- **Intent**: Provide a way to access elements of a collection sequentially without exposing its underlying structure.
- **Structure**: An iterator interface defines methods like `next` and `hasNext`; concrete iterators implement these for specific collections.
- **Example**: A `DinerMenuIterator` allows traversal of a `DinerMenu` array, hiding its internal structure.
- **Key Idea**: Encapsulates collection traversal, making it reusable and flexible.
- **When to Use**: When you need to iterate over different data structures uniformly (e.g., Java’s `Iterator` interface).

### 12. Composite Pattern (Structural)
- **Intent**: Treat individual objects and compositions of objects uniformly.
- **Structure**: A component interface defines operations; leaf classes and composite classes (containing children) implement it.
- **Example**: A `MenuComponent` interface for `MenuItem` (leaf) and `Menu` (composite) to manage restaurant menus hierarchically.
- **Key Idea**: Enables recursive structures where clients treat single items and groups the same way.
- **When to Use**: For hierarchical structures like UI components or file systems.

## Additional Patterns Mentioned Briefly
The book also introduces a few patterns in less detail:
- **State Pattern**: Allows an object to change its behavior when its internal state changes (e.g., a gumball machine with states like `NoQuarter`, `HasQuarter`).
- **Proxy Pattern**: Provides a surrogate or placeholder for another object to control access (e.g., a remote proxy for accessing objects over a network).
- **Compound Patterns**: Combining multiple patterns, like MVC (Model-View-Controller), which uses Observer, Strategy, and Composite.

## Key Takeaways
- **Patterns are solutions to common problems**: They address recurring design challenges in OOP, like flexibility, scalability, and maintainability.
- **Learn the intent and trade-offs**: Understand why a pattern is used and its pros (e.g., flexibility) and cons (e.g., added complexity).
- **Practice with real-world examples**: The book’s strength is its relatable examples (e.g., ducks, pizzas, coffee shops) to ground abstract concepts.
- **Balance simplicity and complexity**: Avoid overusing patterns; apply them only when they solve a specific problem.

## Tips for Applying Design Patterns
1. **Start small**: Use patterns to solve specific problems, not as a default approach.
2. **Refactor to patterns**: Introduce patterns when you notice code smells like tight coupling or duplication.
3. **Combine patterns when needed**: Real-world systems often use multiple patterns (e.g., MVC combines several).
4. **Understand the context**: Patterns depend on the programming language and problem domain (e.g., Java vs. Python).

## Further Resources
- **Book Code Examples**: The book includes Java-based code for each pattern. You can find updated versions or translations (e.g., Python, C#) on GitHub or community sites.
- **Practice Problems**: Try implementing patterns in your projects, such as a coffee shop ordering system (Decorator) or a weather app (Observer).
- **Explore Beyond**: The book covers a subset of the Gang of Four (GoF) patterns. Check *Design Patterns* by Gamma et al. for more (e.g., Bridge, Mediator).