---
icon: pen-clip
---

# @dataclass Decorator

{% hint style="success" %}
* `@dataclass` automatically generates boilerplate code for classes that mainly hold data
* Saves time by auto-generating `__init__`, `__repr__`, `__eq__` methods
* Makes code cleaner and less error-prone
{% endhint %}

### Why I Need This 🤔

Instead of writing this verbose code:

```python
class OldWayPerson:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age
    
    def __repr__(self):
        return f"Person(name={self.name}, age={self.age})"
    
    def __eq__(self, other):
        return self.name == other.name and self.age == other.age
```

write this clean version:

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
```

### Key Features

```ascii
@dataclass
    │
    ├── Automatically generates: 
    │   ├── __init__
    │   ├── __repr__
    │   ├── __eq__
    │   └── __hash__
    │
    ├── Options:
    │   ├── frozen=True  (Immutable)
    │   ├── order=True   (Comparison ops)
    │   └── init=False   (Custom init)
    │
    └── Field options:
        ├── default
        ├── default_factory
        └── init, repr, compare
```

### Cool Examples💡

#### 1. Basic Usage

```python
@dataclass
class Point:
    x: int
    y: int = 0  # Default value

p = Point(10)  # y will be 0
print(p)  # Point(x=10, y=0)
```

#### 2. With Default Factory (for mutable defaults)

{% code overflow="wrap" %}
```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Deck:
    cards: List[str] = field(default_factory=list)  # Correct way to handle mutable defaults!
```
{% endcode %}

#### 3. Frozen (Immutable) Dataclass

```python
@dataclass(frozen=True)
class Config:
    host: str
    port: int = 8080

config = Config("localhost")
# config.port = 9000  # This will raise FrozenInstanceError
```

#### 4. Inheritance

```python
@dataclass
class Animal:
    name: str
    species: str

@dataclass
class Dog(Animal):
    breed: str
    
spot = Dog("Spot", "Canis familiaris", "Dalmatian")
```

### Advanced Tricks 🎩

#### Post-Init Processing

```python
@dataclass
class User:
    name: str
    email: str
    
    def __post_init__(self):
        self.email = self.email.lower()  # Normalize email
```

#### Custom Field Types with Validation

```python
@dataclass
class Temperature:
    celsius: float = field(metadata={'unit': 'C'})
    
    def __post_init__(self):
        if not -273.15 <= self.celsius <= 1000:
            raise ValueError("Invalid temperature!")
```

### When to Use 🎯

```ascii
Use @dataclass when:          Don't use when:
┌─────────────────┐          ┌─────────────────┐
│ • Data Storage  │          │ • Complex Logic │
│ • API Models    │          │ • Many Methods  │
│ • Config Objects│          │ • Dynamic Attrs │
│ • DTOs          │          │ • Metaclasses   │
└─────────────────┘          └─────────────────┘
```

### Pro Tips 💪

1. Use `frozen=True` for immutable objects
2. Always use type hints
3. Use `field(default_factory=list)` for mutable defaults
4. Remember `__post_init__` for custom initialization logic

### Common Gotchas 🚨

* Mutable defaults need `field(default_factory=...)`
* Frozen classes can't be modified after creation
* Order of fields matters in inheritance
* Type hints are for documentation (no runtime validation)

***

### Example Project Structure 🏗️

{% code overflow="wrap" %}
```python
# models.py
from dataclasses import dataclass
from datetime import datetime
from typing import List, Optional

@dataclass
class User:
    id: int
    username: str
    email: str
    created_at: datetime = field(default_factory=datetime.now)
    friends: List[str] = field(default_factory=list)
    nickname: Optional[str] = None

# Usage
user = User(1, "john_doe", "john@example.com")
print(user)  
# User(id=1, username='john_doe', email='john@example.com', created_at=datetime(...), friends=[], nickname=None)
```
{% endcode %}

