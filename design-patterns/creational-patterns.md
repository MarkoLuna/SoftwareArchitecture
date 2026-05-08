# 🏗️ Creational Patterns

Creational design patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.

---

## 1. Singleton Pattern
Ensures a class has only one instance and provides a global access point to it.

### Python Implementation
```python
import threading
from typing import Optional

class Singleton:
    _instance: Optional['Singleton'] = None
    _lock = threading.Lock()

    def __new__(cls) -> 'Singleton':
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
                    cls._instance._initialized = False
        return cls._instance

    def __init__(self):
        if not self._initialized:
            self._data = {}
            self._initialized = True

    def add_data(self, key: str, value: any) -> None:
        self._data[key] = value

    def get_data(self, key: str) -> any:
        return self._data.get(key)

# Usage
singleton1 = Singleton()
singleton2 = Singleton()

print(f"Same instance: {singleton1 is singleton2}")  # True
```

### Java Implementation
```java
import java.util.HashMap;
import java.util.Map;

public class Singleton {
    private static volatile Singleton instance;
    private Map<String, Object> data;
    
    private Singleton() {
        data = new HashMap<>();
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
    
    public void addData(String key, Object value) {
        data.put(key, value);
    }
    
    public Object getData(String key) {
        return data.get(key);
    }
}

// Usage
Singleton singleton1 = Singleton.getInstance();
Singleton singleton2 = Singleton.getInstance();

System.out.println("Same instance: " + (singleton1 == singleton2)); // true
```

### Go Implementation
```go
package singleton

import (
    "sync"
)

type Singleton struct {
    data map[string]interface{}
    mu   sync.RWMutex
}

var (
    instance *Singleton
    once     sync.Once
)

func GetInstance() *Singleton {
    once.Do(func() {
        instance = &Singleton{
            data: make(map[string]interface{}),
        }
    })
    return instance
}

func (s *Singleton) AddData(key string, value interface{}) {
    s.mu.Lock()
    defer s.mu.Unlock()
    s.data[key] = value
}

func (s *Singleton) GetData(key string) interface{} {
    s.mu.RLock()
    defer s.mu.RUnlock()
    return s.data[key]
}
```

---

## 2. Factory Method
Defines an interface for creating an object, but lets subclasses decide which class to instantiate.

### Python Implementation
```python
from abc import ABC, abstractmethod
from typing import Dict, Type

class Animal(ABC):
    @abstractmethod
    def make_sound(self) -> str:
        pass

class Dog(Animal):
    def make_sound(self) -> str:
        return "Woof!"

class Cat(Animal):
    def make_sound(self) -> str:
        return "Meow!"

class AnimalFactory:
    _animals: Dict[str, Type[Animal]] = {
        'dog': Dog,
        'cat': Cat,
    }

    @classmethod
    def create_animal(cls, animal_type: str) -> Animal:
        animal_class = cls._animals.get(animal_type)
        if not animal_class:
            raise ValueError(f"Unknown animal type: {animal_type}")
        return animal_class()
```

### Java Implementation
```java
import java.util.HashMap;
import java.util.Map;

public abstract class Animal {
    public abstract String makeSound();
}

public class Dog extends Animal {
    @Override
    public String makeSound() { return "Woof!"; }
}

public class Cat extends Animal {
    @Override
    public String makeSound() { return "Meow!"; }
}

public class AnimalFactory {
    private static final Map<String, Class<? extends Animal>> animals = new HashMap<>();
    
    static {
        animals.put("dog", Dog.class);
        animals.put("cat", Cat.class);
    }
    
    public static Animal createAnimal(String animalType) {
        Class<? extends Animal> animalClass = animals.get(animalType);
        if (animalClass == null) throw new IllegalArgumentException("Unknown type");
        
        try {
            return animalClass.getDeclaredConstructor().newInstance();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

### Go Implementation
```go
package factory

import "fmt"

type Animal interface {
    MakeSound() string
}

type Dog struct{}
func (d *Dog) MakeSound() string { return "Woof!" }

type Cat struct{}
func (c *Cat) MakeSound() string { return "Meow!" }

type AnimalFactory struct {
    animals map[string]func() Animal
}

func NewAnimalFactory() *AnimalFactory {
    return &AnimalFactory{
        animals: map[string]func() Animal{
            "dog": func() Animal { return &Dog{} },
            "cat": func() Animal { return &Cat{} },
        },
    }
}

func (f *AnimalFactory) CreateAnimal(animalType string) (Animal, error) {
    creator, exists := f.animals[animalType]
    if !exists {
        return nil, fmt.Errorf("unknown animal type")
    }
    return creator(), nil
}
```

---

## 3. Builder Pattern
Constructs complex objects step-by-step. Separates the construction of a complex object from its representation.

### Python Implementation
```python
from dataclasses import dataclass
from typing import Optional

@dataclass
class Computer:
    cpu: str
    memory: str
    storage: str
    gpu: Optional[str] = None

class ComputerBuilder:
    def __init__(self):
        self.computer = Computer("", "", "")

    def set_cpu(self, cpu: str) -> 'ComputerBuilder':
        self.computer.cpu = cpu
        return self

    def set_memory(self, memory: str) -> 'ComputerBuilder':
        self.computer.memory = memory
        return self

    def build(self) -> Computer:
        return self.computer
```

### Java Implementation
```java
public class Computer {
    private String cpu;
    private String memory;

    private Computer(Builder builder) {
        this.cpu = builder.cpu;
        this.memory = builder.memory;
    }

    public static class Builder {
        private String cpu;
        private String memory;

        public Builder setCPU(String cpu) { this.cpu = cpu; return this; }
        public Builder setMemory(String memory) { this.memory = memory; return this; }
        public Computer build() { return new Computer(this); }
    }
}
```

### Go Implementation
```go
package builder

type Computer struct {
    CPU    string
    Memory string
}

type ComputerBuilder struct {
    computer Computer
}

func (b *ComputerBuilder) SetCPU(cpu string) *ComputerBuilder {
    b.computer.CPU = cpu
    return b
}

func (b *ComputerBuilder) Build() Computer {
    return b.computer
}
```

---

## 4. Other Creational Patterns
- **Abstract Factory**: Creates families of related objects without specifying concrete classes.
- **Prototype**: Creates new objects by cloning existing ones.

---

[⬅️ Back to Design Patterns](./README.md)
