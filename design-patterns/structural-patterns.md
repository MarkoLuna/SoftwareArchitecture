# 🧱 Structural Patterns

Structural design patterns explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.

---

## 1. Adapter Pattern
Allows the interface of an existing class to be used as another interface. It connects incompatible interfaces.

### Python Implementation
```python
from abc import ABC, abstractmethod

class MediaPlayer(ABC):
    @abstractmethod
    def play(self, audio_type: str, filename: str) -> None:
        pass

class AdvancedMediaPlayer:
    def play_vlc(self, filename: str) -> None:
        print(f"Playing VLC file: {filename}")

class MediaAdapter(MediaPlayer):
    def __init__(self, audio_type: str):
        self.advanced_player = AdvancedMediaPlayer()

    def play(self, audio_type: str, filename: str) -> None:
        if audio_type.lower() == "vlc":
            self.advanced_player.play_vlc(filename)
```

### Java Implementation
```java
public interface MediaPlayer {
    void play(String audioType, String filename);
}

public class AdvancedMediaPlayer {
    public void playVlc(String filename) {
        System.out.println("Playing VLC file: " + filename);
    }
}

public class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedPlayer = new AdvancedMediaPlayer();
    
    @Override
    public void play(String audioType, String filename) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedPlayer.playVlc(filename);
        }
    }
}
```

### Go Implementation
```go
package adapter

import "fmt"

type MediaPlayer interface {
    Play(audioType, filename string)
}

type AdvancedMediaPlayer struct{}
func (amp *AdvancedMediaPlayer) PlayVlc(filename string) {
    fmt.Printf("Playing VLC file: %s\n", filename)
}

type MediaAdapter struct {
    advancedPlayer *AdvancedMediaPlayer
}

func (ma *MediaAdapter) Play(audioType, filename string) {
    if audioType == "vlc" {
        ma.advancedPlayer.PlayVlc(filename)
    }
}
```

---

## 2. Decorator Pattern
Adds new functionality to an object dynamically without altering its structure.

### Python Implementation
```python
from abc import ABC, abstractmethod

class Coffee(ABC):
    @abstractmethod
    def get_cost(self) -> float:
        pass

class SimpleCoffee(Coffee):
    def get_cost(self) -> float:
        return 2.0

class CoffeeDecorator(Coffee):
    def __init__(self, coffee: Coffee):
        self._coffee = coffee
    def get_cost(self) -> float:
        return self._coffee.get_cost()

class MilkDecorator(CoffeeDecorator):
    def get_cost(self) -> float:
        return self._coffee.get_cost() + 0.5
```

### Java Implementation
```java
public interface Coffee {
    double getCost();
}

public class SimpleCoffee implements Coffee {
    @Override public double getCost() { return 2.0; }
}

public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    public CoffeeDecorator(Coffee coffee) { this.coffee = coffee; }
    @Override public double getCost() { return coffee.getCost(); }
}

public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) { super(coffee); }
    @Override public double getCost() { return coffee.getCost() + 0.5; }
}
```

### Go Implementation
```go
package decorator

type Coffee interface {
    GetCost() float64
}

type SimpleCoffee struct{}
func (sc *SimpleCoffee) GetCost() float64 { return 2.0 }

type CoffeeDecorator struct {
    coffee Coffee
}
func (cd *CoffeeDecorator) GetCost() float64 { return cd.coffee.GetCost() }

type MilkDecorator struct {
    *CoffeeDecorator
}
func (md *MilkDecorator) GetCost() float64 { return md.coffee.GetCost() + 0.5 }
```

---

## 3. Other Structural Patterns
- **Bridge**: Separates an abstraction from its implementation so they can vary independently.
- **Composite**: Composes objects into tree structures to represent part-whole hierarchies.
- **Facade**: Provides a simplified, high-level interface to a complex system.
- **Flyweight**: Reduces memory usage by sharing as much data as possible with similar objects.
- **Proxy**: Provides a placeholder or surrogate for another object to control access to it.

---

[⬅️ Back to Design Patterns](./README.md)
