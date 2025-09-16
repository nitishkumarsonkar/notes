# Facade Pattern (Structural)

The **Facade Pattern** provides a simplified interface to a complex subsystem, making it easier to use. It hides subsystem complexities by offering a higher-level interface, reducing coupling between clients and subsystem components.

## Key Components
1. **Facade**: A class that provides simplified methods to interact with the subsystem (e.g., `HomeTheaterFacade`).
2. **Subsystem Classes**: The complex components that perform the actual work (e.g., `Amplifier`, `DvdPlayer`, etc.).
3. **Client**: Uses the facade to interact with the subsystem without dealing with its complexities.

## Java Code Example: Home Theater

### 1. Subsystem Classes
```java
public class Amplifier {
    public void on() {
        System.out.println("Amplifier on");
    }

    public void setDvd(DvdPlayer dvd) {
        System.out.println("Amplifier setting DVD player");
    }

    public void setVolume(int level) {
        System.out.println("Amplifier setting volume to " + level);
    }

    public void off() {
        System.out.println("Amplifier off");
    }
}

public class DvdPlayer {
    public void on() {
        System.out.println("DVD Player on");
    }

    public void play(String movie) {
        System.out.println("DVD Player playing \"" + movie + "\"");
    }

    public void stop() {
        System.out.println("DVD Player stopped");
    }

    public void off() {
        System.out.println("DVD Player off");
    }
}

public class Projector {
    public void on() {
        System.out.println("Projector on");
    }

    public void setInput(DvdPlayer dvd) {
        System.out.println("Projector setting input to DVD Player");
    }

    public void wideScreenMode() {
        System.out.println("Projector in widescreen mode");
    }

    public void off() {
        System.out.println("Projector off");
    }
}

public class Screen {
    public void down() {
        System.out.println("Screen going down");
    }

    public void up() {
        System.out.println("Screen going up");
    }
}
```

### 2. Facade
```java
public class HomeTheaterFacade {
    private Amplifier amp;
    private DvdPlayer dvd;
    private Projector projector;
    private Screen screen;

    public HomeTheaterFacade(Amplifier amp, DvdPlayer dvd, Projector projector, Screen screen) {
        this.amp = amp;
        this.dvd = dvd;
        this.projector = projector;
        this.screen = screen;
    }

    public void watchMovie(String movie) {
        System.out.println("Get ready to watch a movie...");
        screen.down();
        projector.on();
        projector.setInput(dvd);
        projector.wideScreenMode();
        amp.on();
        amp.setDvd(dvd);
        amp.setVolume(5);
        dvd.on();
        dvd.play(movie);
    }

    public void endMovie() {
        System.out.println("Shutting down the home theater...");
        dvd.stop();
        dvd.off();
        amp.off();
        projector.off();
        screen.up();
    }
}
```

### 3. Client (HomeTheaterTestDrive)
```java
public class HomeTheaterTestDrive {
    public static void main(String[] args) {
        // Create subsystem components
        Amplifier amp = new Amplifier();
        DvdPlayer dvd = new DvdPlayer();
        Projector projector = new Projector();
        Screen screen = new Screen();

        // Create facade
        HomeTheaterFacade homeTheater = new HomeTheaterFacade(amp, dvd, projector, screen);

        // Use facade to watch and end a movie
        homeTheater.watchMovie("Raiders of the Lost Ark");
        System.out.println();
        homeTheater.endMovie();
    }
}
```

## How It Works
1. **Facade**:
   - The `HomeTheaterFacade` provides simplified methods (`watchMovie`, `endMovie`) that coordinate multiple subsystem components.

2. **Subsystem**:
   - Classes like `Amplifier`, `DvdPlayer`, `Projector`, and `Screen` perform specific tasks but have complex interactions.

3. **Client Interaction**:
   - The client uses the facade’s high-level methods, avoiding direct interaction with the subsystem’s complexity.

4. **Decoupling**:
   - The facade reduces coupling by acting as a single point of access, shielding the client from subsystem details.

## Output of HomeTheaterTestDrive
```
Get ready to watch a movie...
Screen going down
Projector on
Projector setting input to DVD Player
Projector in widescreen mode
Amplifier on
Amplifier setting DVD player
Amplifier setting volume to 5
DVD Player on
DVD Player playing "Raiders of the Lost Ark"

Shutting down the home theater...
DVD Player stopped
DVD Player off
Amplifier off
Projector off
Screen going up
```

## When to Use the Facade Pattern
- When you want to simplify interactions with a complex subsystem.
- When you need a unified interface for a set of classes in a subsystem.
- Examples: Simplifying APIs, library interfaces, or complex system interactions (e.g., home automation systems).

## Advantages
- Simplifies client code by providing a high-level interface.
- Reduces coupling between clients and subsystem components.
- Makes subsystems easier to use and maintain.

## Disadvantages
- The facade may become a "god object" if it handles too many responsibilities.
- May limit access to advanced subsystem features if not designed carefully.
- Adds an extra layer, which can increase complexity for small systems.

## Real-World Analogy
Think of a car’s dashboard (facade) that simplifies driving by providing controls like the ignition and accelerator, hiding the complexity of the engine, transmission, and other systems.