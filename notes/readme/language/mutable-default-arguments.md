---
icon: pizza
---

# Mutable Default Arguments

### The Problem

When a mutable object is used as a default argument, that object is created only _once_ at the time of function definition. This means that if the function modifies the default argument, those modifications persist across subsequent calls to the function, leading to unexpected and often hard-to-debug behavior.

### Example

```python
def add_item(item, my_list=[]):  # Mutable default argument (list)
    my_list.append(item)
    return my_list

print(add_item(1))  # Output: [1] (Expected)
print(add_item(2))  # Output: [1, 2] (Unexpected!)
print(add_item(3))  # Output: [1, 2, 3] (Unexpected!)

print(add_item(4, []))  # Output: [4] (Expected, because we provided a new list)
```

In the above example, the first call to `add_item(1)` behaves as expected, appending `1` to the initially empty list. However, the second call `add_item(2)` appends `2` to the _same_ list that was modified in the first call. This continues for subsequent calls, resulting in the list accumulating all the added items. Only when we explicitly provide a new list as the second argument (`add_item(4, [])`) do we get the expected behaviour of a fresh, empty list.

### Why This Happens <a href="#why-this-happens" id="why-this-happens"></a>

The default argument is evaluated only once when the function is defined, not each time the function is called. This creates a single instance of the mutable object that is shared across all calls to the function if the argument is not explicitly provided.

### The Solution: Use `None` and Initialise Inside <a href="#the-solution-use-none-and-initialize-inside" id="the-solution-use-none-and-initialize-inside"></a>

The standard and recommended way to avoid this issue is to use `None` as the default argument and then initialise the mutable object _inside_ the function body:

```python
def add_item(item, my_list=None):  # Use None as the default
    if my_list is None:
        my_list = []  # Create a new list each time the function is called
    my_list.append(item)
    return my_list
print(add_item(1))  # Output: [1]
print(add_item(2))  # Output: [2] (Now correct!)
print(add_item(3))  # Output: [3] (Now correct!)
```

By initialising the list inside the function, we ensure that a new list is created every time the function is called (if the argument is not provided), preventing the unintended sharing of state between calls.

### Key Takeaway <a href="#key-takeaway" id="key-takeaway"></a>

Always use `None` as the default argument when the default value needs to be a mutable object. This practice helps avoid subtle bugs and ensures that your functions behave predictably. This applies to lists, dictionaries, sets, and any other mutable objects in Python.

