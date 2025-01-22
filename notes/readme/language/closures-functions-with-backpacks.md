---
icon: backpack
---

# Closures: Functions with Backpacks

{% hint style="info" %}
A closure is a function that "remembers" its creation environment, even after the outer function completes. It "closes over" free variables. Useful for data encapsulation, state preservation, decorators, and partial application.
{% endhint %}

**What is a Closure?**

A closure is an inner function that retains access to variables from its enclosing scope (the outer function's scope), even after the outer function has finished executing. This "remembering" is achieved through cell objects that hold the values of free variables. A free variable is a variable used within a function but not defined within that function's local scope.

**How Closures Work**

1. **Nested Function:** A closure is always an inner function defined within an outer function.
2. **Free Variables:** The inner function references variables from the outer function's scope (free variables).
3. **Returning the Inner Function:** The outer function _returns_ the inner function.
4. **Closure Created:** When the outer function is called, it creates and returns an instance of the inner function. This returned inner function is a closure. It holds a reference to the cell objects containing the values of the free variables.
5. **Accessing Free Variables:** Even after the outer function completes, the closure can still access the values of the free variables through the cell objects.

**Example**

```python
def outer_function(x):
    def inner_function(y):
        return x + y  # x is a free variable
    return inner_function

add_5 = outer_function(5)   # add_5 is a closure
print(add_5(3))           # Output: 8 (add_5 remembers x = 5)

add_10 = outer_function(10)  # add_10 is another closure
print(add_10(3))          # Output: 13 (add_10 remembers x = 10)
```

**Key Characteristics**

* **Encapsulation:** Closures encapsulate state and behavior, promoting code organization.
* **State Preservation:** Closures can maintain state across multiple calls without using global variables.
* **Immutability (by default):** Captured variables are immutable unless the `nonlocal` keyword is used.
* **Delayed Execution:** Closures enable partial application (currying) and deferred execution.

**Benefits and Use Cases**

* **Data Encapsulation/Hiding:** Create functions with private state (like counters or configuration settings).
* **Function Factories:** Generate functions with pre-configured parameters.
* **Decorators:** Extend function behavior without modification.
* **Callbacks and Event Handling:** Pass functions with pre-set context or state.
* **Partial Application (Currying):** Create specialized functions by pre-filling arguments.
* **Avoiding Global Variables:** Provide a cleaner alternative to globals for state management.

**Example: Function Factory**

```python
def make_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))  # Output: 10
print(triple(5))  # Output: 15
```

Function factory _uses_ a closure to generate specialised functions. The factory itself doesn't represent a different _kind_ of closure; it's a closure with a specific _role_.

**Example: Data Encapsulation**

```python
def create_counter():
    count = 0
    def increment():
        nonlocal count  # Needed to modify count
        count += 1
        return count
    return increment
my_counter = create_counter()
print(my_counter())  # 1
print(my_counter())  # 2
```

