# Observer Pattern (Behavioral)

The **Observer Pattern** defines a one-to-many dependency where multiple objects (observers) are notified of state changes in a subject. It promotes loose coupling, allowing observers to be added or removed dynamically without modifying the subject.

## Key Components
1. **Subject Interface**: Defines methods to register, remove, and notify observers.
2. **Concrete Subject**: Maintains a list of observers and notifies them of state changes.
3. **Observer Interface**: Defines the update method that observers implement.
4. **Concrete Observers**: Implement the observer interface to react to subject changes.
5. **Client**: Sets up the subject and observers, triggering updates.

## Java Code Example: Weather Station

### 1. Subject Interface
```java
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```

### 2. Observer Interface
```java
public interface Observer {
    void update(float temperature, float humidity, float pressure);
}
```

### 3. Concrete Subject (WeatherData)
```java
import java.util.ArrayList;
import java.util.List;

public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<>();
    }

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    // Called when weather measurements change
    public void measurementsChanged() {
        notifyObservers();
    }

    // Simulate new weather data
    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
}
```

### 4. Concrete Observer (CurrentConditionsDisplay)
```java
public class CurrentConditionsDisplay implements Observer {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}
```

### 5. Concrete Observer (StatisticsDisplay)
```java
public class StatisticsDisplay implements Observer {
    private float maxTemp = 0.0f;
    private float minTemp = 200.0f;
    private float tempSum = 0.0f;
    private int numReadings;
    private Subject weatherData;

    public StatisticsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity, float pressure) {
        tempSum += temperature;
        numReadings++;

        if (temperature > maxTemp) {
            maxTemp = temperature;
        }
        if (temperature < minTemp) {
            minTemp = temperature;
        }

        display();
    }

    public void display() {
        System.out.println("Avg/Max/Min temperature: " + (tempSum / numReadings) + "/" + maxTemp + "/" + minTemp);
    }
}
```

### 6. Client (WeatherStation)
```java
public class WeatherStation {
    public static void main(String[] args) {
        // Create the subject
        WeatherData weatherData = new WeatherData();

        // Create observers
        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay(weatherData);
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);

        // Simulate new weather measurements
        weatherData.setMeasurements(80, 65, 30.4f);
        weatherData.setMeasurements(82, 70, 29.2f);
        weatherData.setMeasurements(78, 90, 29.2f);

        // Remove an observer and update again
        weatherData.removeObserver(statisticsDisplay);
        System.out.println("--- After removing StatisticsDisplay ---");
        weatherData.setMeasurements(75, 60, 30.0f);
    }
}
```

## How It Works
1. **Subject-Observer Relationship**:
   - `WeatherData` (subject) maintains a list of `Observer` objects and notifies them when its state changes.
   - Observers (`CurrentConditionsDisplay`, `StatisticsDisplay`) register with the subject to receive updates.

2. **Push vs. Pull**:
   - This example uses a **push** approach, where the subject sends data (`temperature`, `humidity`, `pressure`) to observers.
   - Alternatively, a **pull** approach could have observers query the subject for data.

3. **Dynamic Registration**:
   - Observers can be added or removed at runtime using `registerObserver` and `removeObserver`.

4. **Loose Coupling**:
   - The subject doesn’t know the concrete classes of observers, only the `Observer` interface.
   - Observers depend on the subject’s interface, not its implementation.

## Output of WeatherStation
```
Current conditions: 80.0F degrees and 65.0% humidity
Avg/Max/Min temperature: 80.0/80.0/80.0
Current conditions: 82.0F degrees and 70.0% humidity
Avg/Max/Min temperature: 81.0/82.0/80.0
Current conditions: 78.0F degrees and 90.0% humidity
Avg/Max/Min temperature: 80.0/82.0/78.0
--- After removing StatisticsDisplay ---
Current conditions: 75.0F degrees and 60.0% humidity
```

## When to Use the Observer Pattern
- When multiple objects need to react to state changes in another object (e.g., UI updates, event handling).
- When you need a one-to-many relationship with dynamic subscriptions.
- Examples: GUI frameworks (e.g., Java’s `java.util.Observable`), pub/sub systems, data feeds.

## Advantages
- Promotes loose coupling between subject and observers.
- Supports dynamic addition/removal of observers.
- Enables broadcast communication to multiple objects.

## Disadvantages
- Can lead to memory leaks if observers aren’t properly deregistered.
- Notification overhead increases with many observers.
- Observers may receive unnecessary updates in a push model.

## Real-World Analogy
Think of a news agency (subject) that broadcasts updates to subscribers (observers) like newspapers or apps. Subscribers can join or leave without affecting the agency.