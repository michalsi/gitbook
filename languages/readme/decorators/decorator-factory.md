---
icon: industry-windows
---

# Decorator Factory

Decorators are a powerful feature in Python that allows you to modify the behavior of functions or methods. A decorator factory is an advanced concept that offers even more flexibility by enabling parameterization of decorators. Here's a simple guide to help you grasp this concept.

{% hint style="info" %}
### TL;DR&#x20;

* **Decorator**: Enhances function behavior without altering its structure.
* **Decorator Factory**: A function that returns a decorator, allowing decorators to accept parameters.
* **Use Case**: Ideal for customizing decorators to make them versatile and reusable.
* **Example**: Create a decorator to repeat function calls a specified number of times.
{% endhint %}

***

### What is a Decorator Factory?

A decorator factory is essentially a function that returns a decorator function. This allows you to pass parameters to your decorator, making it adaptable for different scenarios.

#### Why Use Decorator Factories? 🤔

* **Customisation**: Tailor the behaviour of your decorator by passing arguments.
* **Reusability**: Apply the same decorator logic with different configurations, maximising code reuse.

### Example of a Decorator Factory

```python
def repeat(num_times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator
    
# Applying the decorator factory
@repeat(num_times=3)
def greet(name):
    print(f"Hello, {name}!")

# Function call
greet("Alice")

# Output
Hello, Alice!
Hello, Alice!
Hello, Alice!
```

#### Explanation <a href="#explanation" id="explanation"></a>

1. `repeat(num_times)`: The factory function that takes `num_times` as a parameter, specifying the number of repetitions.
2. `decorator(func)`: The actual decorator function that is returned by the factory.
3. `wrapper(*args, **kwargs)`: The inner function that wraps the original function, executing it `num_times` times.
4. **Applying the Decorator**: The decorator is applied using `@repeat(num_times=3)`, calling the factory and wrapping `greet`.
5. **Execution**: When `greet("Alice")` is called, it prints the greeting three times.

### Conclusion  <a href="#conclusion" id="conclusion"></a>

Decorator factories add a layer of flexibility to decorators, making them a valuable tool for Python developers who want to write cleaner, more adaptable code.
