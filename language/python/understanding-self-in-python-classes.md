---
icon: head-side-goggles
---

# Understanding self in Python Classes

{% hint style="info" %}
#### **TL;DR:**  <a href="#understanding-self-in-python-classes" id="understanding-self-in-python-classes"></a>

In Python, `self` is a convention used in instance methods to refer to the object itself. It allows access to instance attributes and methods. Although not a reserved keyword, using `self` ensures consistency and readability in your code.
{% endhint %}

***

**The Basics of Python Classes**

In Python, a class is a blueprint for creating objects. It encapsulates data (attributes) and behaviors (methods) that operate on the data. When you define a class, you typically define an `__init__` method to initialize your objects, and you'll often see `self` as the first parameter in instance methods.

**Why `self`? 🤔**

* **Convention, Not a Keyword:** While `self` is not a reserved keyword, it's a widely adopted convention in Python programming. It signifies that the method belongs to the instance of the class.
* **Instance Reference:** `self` refers to the specific instance of the class, allowing methods to access and modify the object's attributes and call other methods.

**How It Works 🔍**

1. **Instantiation:**

```python
class MyClass:
    def __init__(self, value):
        self.value = value
```

* When you create an instance (`obj = MyClass(10)`), Python automatically passes the instance (`obj`) as the first argument to the `__init__` method.

1. **Accessing Attributes and Methods:**

```python
def display_value(self): 
    print(self.value)
```

* Here, `self.value` allows `display_value` to access the `value` attribute of the specific instance.

**Example in Action 🎬**

```python
class Triangle:
    def __init__(self, width, height, print_char):
        self.width = width
        self.height = height
        self.print_char = print_char

    def print_right_angle(self):
        for i in range(1, self.height + 1):
            print(self.print_char * i)

# Create a triangle instance
triangle = Triangle(4, 4, '*')

# Call a method
triangle.print_right_angle()
```

* **Method Calls:** When `triangle.print_right_angle()` is called, it translates to `Triangle.print_right_angle(triangle)`, with `triangle` as `self`.

**Key Takeaways 📝**

* **Consistency Matters:** Using `self` ensures your code is consistent and easily readable by others familiar with Python's conventions.
* **Flexibility and Power:** Understanding `self` empowers you to build robust classes that can maintain state and behaviour across different instances.

In conclusion, while `self` is just a convention, it plays a crucial role in the organisation and functionality of Python code. Embracing this convention helps maintain clarity and consistency, ensuring your code is both powerful and easy to understand.
