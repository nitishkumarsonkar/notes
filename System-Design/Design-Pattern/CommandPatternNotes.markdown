# Command Pattern (Behavioral)

The **Command Pattern** encapsulates a request as an object, allowing parameterization of clients with different requests, queuing, logging, or support for undoable operations. It decouples the sender (invoker) from the receiver that performs the action.

## Key Components
1. **Command Interface**: Defines the interface for executing commands (e.g., `Command` with `execute()`).
2. **Concrete Commands**: Implement the command interface, binding a receiver to an action (e.g., `LightOnCommand`).
3. **Receiver**: Knows how to perform the action (e.g., `Light`).
4. **Invoker**: Triggers the command execution (e.g., `RemoteControl`).
5. **Client**: Configures the commands and assigns them to the invoker.

## Java Code Example: Remote Control

### 1. Command Interface
```java
public interface Command {
    void execute();
}
```

### 2. Receiver Classes
```java
public class Light {
    private String location;

    public Light(String location) {
        this.location = location;
    }

    public void on() {
        System.out.println(location + " Light is On");
    }

    public void off() {
        System.out.println(location + " Light is Off");
    }
}

public class CeilingFan {
    private String location;
    public static final int HIGH = 3;
    public static final int MEDIUM = 2;
    public static final int LOW = 1;
    public static final int OFF = 0;
    private int speed;

    public CeilingFan(String location) {
        this.location = location;
        speed = OFF;
    }

    public void high() {
        speed = HIGH;
        System.out.println(location + " Ceiling Fan is on High");
    }

    public void off() {
        speed = OFF;
        System.out.println(location + " Ceiling Fan is Off");
    }

    public int getSpeed() {
        return speed;
    }
}
```

### 3. Concrete Commands
```java
public class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.on();
    }
}

public class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.off();
    }
}

public class CeilingFanHighCommand implements Command {
    private CeilingFan ceilingFan;

    public CeilingFanHighCommand(CeilingFan ceilingFan) {
        this.ceilingFan = ceilingFan;
    }

    public void execute() {
        ceilingFan.high();
    }
}
```

### 4. Invoker (RemoteControl)
```java
public class RemoteControl {
    private Command[] onCommands;
    private Command[] offCommands;

    public RemoteControl() {
        onCommands = new Command[7];
        offCommands = new Command[7];
        Command noCommand = new NoCommand();
        for (int i = 0; i < 7; i++) {
            onCommands[i] = noCommand;
            offCommands[i] = noCommand;
        }
    }

    public void setCommand(int slot, Command onCommand, Command offCommand) {
        onCommands[slot] = onCommand;
        offCommands[slot] = offCommand;
    }

    public void onButtonWasPushed(int slot) {
        onCommands[slot].execute();
    }

    public void offButtonWasPushed(int slot) {
        offCommands[slot].execute();
    }
}

class NoCommand implements Command {
    public void execute() {
        System.out.println("No command assigned");
    }
}
```

### 5. Client (RemoteControlTest)
```java
public class RemoteControlTest {
    public static void main(String[] args) {
        // Create invoker
        RemoteControl remoteControl = new RemoteControl();

        // Create receivers
        Light livingRoomLight = new Light("Living Room");
        CeilingFan ceilingFan = new CeilingFan("Living Room");

        // Create commands
        LightOnCommand livingRoomLightOn = new LightOnCommand(livingRoomLight);
        LightOffCommand livingRoomLightOff = new LightOffCommand(livingRoomLight);
        CeilingFanHighCommand ceilingFanHigh = new CeilingFanHighCommand(ceilingFan);

        // Assign commands to invoker
        remoteControl.setCommand(0, livingRoomLightOn, livingRoomLightOff);
        remoteControl.setCommand(1, ceilingFanHigh, new NoCommand());

        // Test commands
        remoteControl.onButtonWasPushed(0);
        remoteControl.offButtonWasPushed(0);
        remoteControl.onButtonWasPushed(1);
        remoteControl.offButtonWasPushed(1);
    }
}
```

## How It Works
1. **Command Encapsulation**:
   - Each command (e.g., `LightOnCommand`) encapsulates a request by binding a receiver (`Light`) to an action (`on()`).

2. **Invoker**:
   - The `RemoteControl` holds commands in slots and triggers them via `onButtonWasPushed` or `offButtonWasPushed`.

3. **Decoupling**:
   - The invoker doesn’t know how the receiver performs the action, only that it calls `execute()` on a command.

4. **Extensibility**:
   - New commands can be added without changing the invoker or receivers (e.g., adding `CeilingFanHighCommand`).

## Output of RemoteControlTest
```
Living Room Light is On
Living Room Light is Off
Living Room Ceiling Fan is on High
No command assigned
```

## When to Use the Command Pattern
- When you need to decouple the requester of an action from the object performing it.
- When you want to support undo/redo, queuing, or logging of operations.
- Examples: GUI button actions, task scheduling, or transaction systems.

## Advantages
- Decouples invoker from receiver, promoting loose coupling.
- Supports undo/redo by adding an `undo()` method to the command interface.
- Allows queuing or logging of commands for later execution.

## Disadvantages
- Increases the number of classes (one per command).
- Can add complexity for simple operations.
- May require additional logic for managing command history (e.g., for undo).

## Real-World Analogy
Think of a restaurant waiter (invoker) taking orders (commands) from customers (client) and passing them to the kitchen (receiver). The waiter doesn’t need to know how the kitchen prepares the food, only how to deliver the order.