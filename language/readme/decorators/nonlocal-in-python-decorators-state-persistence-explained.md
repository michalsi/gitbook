---
icon: ball-pile
---

# \`nonlocal\` in Python Decorators: State Persistence Explained

{% hint style="info" %}
#### TL;DR:

* 🌀 **Single Instance:** Decorators create a single wrapper instance, not a new one per call.
* 🔒 **Closure Persistence:** Captures and retains variables from enclosing scopes using closures.
* 🔄 **Stateful Calls:** `nonlocal` allows state retention across multiple function calls.
{% endhint %}

***

Decorators in Python are not just syntactic sugar; they are powerful tools that can enhance or alter the behavior of functions. A key feature that enables this power is the use of closures and `nonlocal` variables.&#x20;

#### How Decorators and Closures Work Together

1. **Function Definition vs. Execution:**
   * When you define a function with a decorator, the decorator is applied at definition time, wrapping the original function with additional logic.
   * This process creates a new function object (the wrapper), replacing the original function in the namespace.
2. **Single Instance of Decorator Logic:**
   * The decorator logic is instantiated once. It creates a closure around any `nonlocal` variables, capturing their state.
   * This means the same instance of the wrapper function is used every time the decorated function is called, retaining any state changes.
3. **Closure Property:**
   * A closure is a function that retains access to its lexical scope, even when the function is invoked outside that scope.
   * In decorators, this means the `wrapper` function can access and modify `nonlocal` variables captured at decoration time.

#### Why Does the State Persist?

* **Shared State:** The `nonlocal` variable is part of the persistent environment of the closure, allowing it to maintain changes across calls.
* **Persistent Environment:** The closure environment is preserved for the life of the decorated function, enabling state accumulation.

#### Example to Illustrate

Here's a practical example using a call counter:

```python
def call_counter(func):
    count = 0  # Enclosing scope variable
    def wrapper(*args, **kwargs):
        nonlocal count
        count += 1
        print(f"{func.__name__} has been called {count} times")
        return func(*args, **kwargs)
    return wrapper
@call_counter
def example_function():
    print("Function execution")
# Each call to example_function uses the same wrapper instance
example_function()  # Output: Function execution
                    #         example_function has been called 1 times
example_function()  # Output: Function execution
                    #         example_function has been called 2 times\]
```

#### Key Takeaways <a href="#key-takeaways" id="key-takeaways"></a>

* **Single Decorator Instance:** The decorator logic is applied once, creating a persistent wrapper function.
* **State Retention:** The `nonlocal` variable retains its state between calls due to the closure capturing it.
