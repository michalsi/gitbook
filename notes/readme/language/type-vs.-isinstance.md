---
icon: down-left-and-up-right-to-center
---

# type() vs. isinstance()

{% hint style="info" %}
#### TL;DR: `type()` vs. `isinstance()` <a href="#tl-dr-type-vs.-isinstance" id="tl-dr-type-vs.-isinstance"></a>

* **Flexibility:** `isinstance()` supports subclass checks; `type()` does not.
* **Polymorphism:** `isinstance()` is more Pythonic, recognising inherited types.
* **Extensibility:** Easily check multiple types with `isinstance()`.
* **Readability:** `isinstance()` is conventionally preferred for clarity.
{% endhint %}

***

**Detailed Breakdown:**

`type() == int`:

* Checks if an object is exactly of type `int`.
* Doesn't work with subclasses. If `num` is from a subclass, it won't pass this check.
* Use this when you need strict type matching.

`isinstance(num, int)`:

* Recognises both `int` and its subclasses. Handy for object-oriented scenarios.
* The Python community prefers this for its simplicity and readability.
* Allows checking against multiple types like `isinstance(num, (int, float))`.

**Example with Subclasses:**

```python
class MyInt(int):
    pass

my_num = MyInt(5)

# This will be False because MyInt is not exactly int
print(type(my_num) == int)  # Output: False

# This will be True because MyInt is a subclass of int
print(isinstance(my_num, int))  # Output: True
```

**Bottom Line:**

* Go with `isinstance()` for more flexible and maintainable code.
* It's the tool of choice for dynamic, object-oriented programming in Python.

\
