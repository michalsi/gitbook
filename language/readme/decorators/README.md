# 🎍 Decorators

A decorator in Python is a design pattern that allows you to modify the behavior of a function or class method. Decorators are a powerful tool for extending functionalities without changing the actual code of the function being decorated.

### Basic Usage

Decorators are typically defined as functions that take another function as an argument, add some functionality, and return a new function. You apply a decorator to a function by prefixing the function definition with the decorator name, preceded by the `@` symbol.

#### Example

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper
    
@my_decorator
def say_hello():
    print("Hello!")
    
say_hello()
```

**Output:**

```
Something is happening before the function is called.
Hello!
Something is happening after the function is called.
```

### What Happens Under the Hood? <a href="#what-happens-under-the-hood" id="what-happens-under-the-hood"></a>

When a decorator is applied to a function, the following steps occur:

1. **Function Reference:** The function to be decorated is passed as an argument to the decorator function.
   1. `say_hello = my_decorator(say_hello)`
2. **Wrapper Function:** The decorator defines a nested `wrapper` function which includes additional behavior (such as logging or validation) along with the original function's behavior.
3. **Replacement:** The original function reference is replaced with the `wrapper` function. This means that when you call the decorated function, you are actually calling the `wrapper` function.
4. **Execution:** The `wrapper` function executes any pre-processing code, calls the original function, and then executes any post-processing code.

### Key Points

* **Function Wrapping:** Decorators wrap a function, allowing pre-processing and post-processing around the wrapped function's execution.
* **Reusability:** They promote code reusability by allowing the same decorator to be applied to multiple functions.
* **@ Syntax:** The `@` syntax is syntactic sugar

### When to Use Decorators <a href="#when-to-use-decorators" id="when-to-use-decorators"></a>

* **Logging:** Automatically add logging to functions.
* **Access Control:** Implement access control and authentication.
* **Caching:** Cache results of expensive function calls.
* **Validation:** Perform validation on input arguments.

\
