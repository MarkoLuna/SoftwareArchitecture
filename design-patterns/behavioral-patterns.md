# 🧠 Behavioral Patterns

Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects.

---

## 1. Observer Pattern
Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

### Python Implementation
```python
from typing import List, Protocol

class Observer(Protocol):
    def update(self, temperature: float) -> None:
        pass

class WeatherStation:
    def __init__(self):
        self._observers: List[Observer] = []
        self._temp = 0.0

    def register(self, observer: Observer):
        self._observers.append(observer)

    def notify(self):
        for obs in self._observers:
            obs.update(self._temp)
```

### Java Implementation
```java
import java.util.ArrayList;
import java.util.List;

public interface Observer {
    void update(float temperature);
}

public class WeatherStation {
    private List<Observer> observers = new ArrayList<>();
    private float temperature;

    public void register(Observer o) { observers.add(o); }
    public void notifyObservers() {
        for (Observer o : observers) o.update(temperature);
    }
}
```

### Go Implementation
```go
package observer

type Observer interface {
    Update(temp float64)
}

type WeatherStation struct {
    observers []Observer
    temp      float64
}

func (ws *WeatherStation) Register(o Observer) {
    ws.observers = append(ws.observers, o)
}

func (ws *WeatherStation) Notify() {
    for _, o := range ws.observers {
        o.Update(ws.temp)
    }
}
```

---

## 2. Strategy Pattern
Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

### Python Implementation
```python
from abc import ABC, abstractmethod

class PaymentStrategy(ABC):
    @abstractmethod
    def pay(self, amount: float) -> bool:
        pass

class CreditCardPayment(PaymentStrategy):
    def pay(self, amount: float) -> bool:
        print(f"Paid ${amount} with Credit Card")
        return True
```

### Java Implementation
```java
public interface PaymentStrategy {
    boolean pay(double amount);
}

public class CreditCardPayment implements PaymentStrategy {
    @Override
    public boolean pay(double amount) {
        System.out.println("Paid " + amount + " with CC");
        return true;
    }
}
```

### Go Implementation
```go
package strategy

import "fmt"

type PaymentStrategy interface {
    Pay(amount float64) bool
}

type CreditCardPayment struct{}
func (ccp *CreditCardPayment) Pay(amount float64) bool {
    fmt.Printf("Paid %f with CC\n", amount)
    return true
}
```

---

## 3. Other Behavioral Patterns
- **Chain of Responsibility**: Passes requests along a chain of handlers.
- **Command**: Encapsulates a request as an object, allowing parameterization of clients.
- **Iterator**: Accesses elements of a collection sequentially without exposing its underlying representation.
- **Mediator**: Simplifies communication between objects by directing them through a central hub.
- **Memento**: Captures and restores an object's internal state.
- **State**: Allows an object to alter its behavior when its internal state changes.
- **Template Method**: Defines the skeleton of an algorithm, leaving some steps to subclasses.
- **Visitor**: Separates an algorithm from the object structure on which it operates.

---

[⬅️ Back to Design Patterns](./README.md)
