# SOLID Principle  
https://moonshot.scaler.com/s/sl/s6aztSTsoS

1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov's Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DSP)

## Single Responsibility Principle (SRP)

1. Any function/class/module/package (unit-of-code) should have only 1, well-defined responsibility. 
2. If some piece of code doesn't follow the SRP, then we should split it into multiple parts.
3. OOP - provides a way of decomposing complex things: inheritance. 

### Example of SRP

```java
// Bad Example - Multiple Responsibilities
class Employee {
    void calculatePay() { /* ... */ }
    void saveToDatabase() { /* ... */ }
    void generateReport() { /* ... */ }
}

// Good Example - Single Responsibility
class PayCalculator {
    void calculatePay() { /* ... */ }
}

class EmployeeRepository {
    void saveEmployee() { /* ... */ }
}

class ReportGenerator {
    void generateReport() { /* ... */ }
}
```

In the good example, each class has a single, well-defined responsibility:
- `PayCalculator`: Handles salary calculations
- `EmployeeRepository`: Manages database operations
- `ReportGenerator`: Generates reports

This makes the code:
- Easier to maintain
- More flexible to change
- Simpler to test
- More reusable

## Open/Closed Principle (OCP)

A class should be open for extension but closed for modification. This means you can change the behavior of a class by adding new code (e.g., through inheritance or composition) rather than altering the existing code. Following OCP helps keep the code base stable and minimizes the risk of introducing new bugs.

### Example of OCP in Java

```java
// Bad Example - Violates OCP
class GraphicEditor {
    public void drawShape(Object shape) {
        if (shape instanceof Circle) {
            drawCircle((Circle) shape);
        } else if (shape instanceof Rectangle) {
            drawRectangle((Rectangle) shape);
        }
        // Each time a new shape is added, this method must be modified.
    }
    
    private void drawCircle(Circle circle) {
        // Drawing logic for Circle
    }
    
    private void drawRectangle(Rectangle rectangle) {
        // Drawing logic for Rectangle
    }
}

class Circle {
    // Circle properties and methods
}

class Rectangle {
    // Rectangle properties and methods
}
// Good Example - Follows OCP
interface Shape {
    void draw();
}

class Circle implements Shape {
    @Override
    public void draw() {
        // Drawing logic for Circle
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        // Drawing logic for Rectangle
    }
}

class GraphicEditor {
    public void drawShape(Shape shape) {
        shape.draw(); // New shapes can be added without modifying this method.
    }
}
```
##  Liskov Substitution Principle (LSP)

Objects of a superclass should be replaceable with objects of its subclasses without breaking the application. In other words, what we can do with a base class should also be possible with its derived class.

### Example of LSP

```java
// Bad Example - Violates LSP
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) {
        this.width = width;
    }
    
    public void setHeight(int height) {
        this.height = height;
    }
    
    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }
    
    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}

// Good Example - Follows LSP
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    private int width;
    private int height;
    
    public void setWidth(int width) {
        this.width = width;
    }
    
    public void setHeight(int height) {
        this.height = height;
    }
    
    @Override
    public int getArea() {
        return width * height;
    }
}

class Square implements Shape {
    private int side;
    
    public void setSide(int side) {
        this.side = side;
    }
    
    @Override
    public int getArea() {
        return side * side;
    }
}
```

In the bad example:
- Square extends Rectangle but violates LSP because it changes the behavior of setWidth/setHeight
- Code that works with Rectangle might break when using Square

In the good example:
- Both shapes implement a common interface
- Each shape has its own appropriate properties and methods
- Code using Shape interface works correctly with both implementations

This makes the code:
- More robust and reliable
- Truly polymorphic
- Easier to extend with new shapes
- Maintains expected behaviors
## Interface Segregation Principle (ISP)

Clients should not be forced to depend on interfaces they don't use. This principle suggests breaking down large interfaces into smaller, more specific ones.

### Example of ISP

```java
// Bad Example - Violates ISP
interface Animal {
    void fly();
    void swim();
    void run();
}

class Duck implements Animal {
    public void fly() { /* Can fly */ }
    public void swim() { /* Can swim */ }
    public void run() { /* Can run */ }
}

class Fish implements Animal {
    public void fly() { /* Cannot fly! */ }
    public void swim() { /* Can swim */ }
    public void run() { /* Cannot run! */ }
}

// Good Example - Follows ISP
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

interface Runnable {
    void run();
}

class Duck implements Flyable, Swimmable, Runnable {
    public void fly() { /* Can fly */ }
    public void swim() { /* Can swim */ }
    public void run() { /* Can run */ }
}

class Fish implements Swimmable {
    public void swim() { /* Can swim */ }
}
```

In the good example:
- Interfaces are segregated by animal capabilities
- Animals only implement interfaces for abilities they have
- Fish isn't forced to implement fly() or run()
- Code is more logical and maintainable

Benefits:
- More focused interfaces
- No unnecessary method implementations
- Better represents real-world capabilities
- Easier to extend and maintain

## Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions.

### Example of DIP

```java
// Bad Example - Violates DIP
class EmailService {
    public void sendEmail(String message) {
        // Email sending logic
    }
}

class NotificationService {
    private EmailService emailService = new EmailService();
    
    public void sendNotification(String message) {
        emailService.sendEmail(message);
    }
}

// Good Example - Follows DIP
interface MessageService {
    void sendMessage(String message);
}

class EmailService implements MessageService {
    @Override
    public void sendMessage(String message) {
        // Email sending logic
    }
}

class SMSService implements MessageService {
    @Override
    public void sendMessage(String message) {
        // SMS sending logic
    }
}

class NotificationService {
    private MessageService messageService;
    
    public NotificationService(MessageService service) {
        this.messageService = service;
    }
    
    public void sendNotification(String message) {
        messageService.sendMessage(message);
    }
}
```

In the good example:
- High-level NotificationService depends on MessageService abstraction
- Low-level EmailService and SMSService implement the abstraction
- NotificationService is flexible and can work with any MessageService implementation

Benefits:
- Decoupled code
- Easier to test (can mock interfaces)
- More flexible architecture
- Simplified maintenance