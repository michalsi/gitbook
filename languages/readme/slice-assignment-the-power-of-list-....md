---
icon: knife-kitchen
---

# Slice Assignment: The Power of list\[:] = ...

{% hint style="success" %}
The `list[:] = ...` syntax in Python modifies a list in-place. It's particularly useful when working with functions that reference the original list (like `os.walk()`).&#x20;

This idiom maintains existing references while updating the list's contents efficiently.
{% endhint %}

### Deep Dive 🕵️‍♀️ <a href="#deep-dive" id="deep-dive"></a>

In Python, `list[:] = ...` is a powerful idiom that allows you to modify a list in-place. Let's break it down:

1. `[:]` is slice notation, typically used to create a shallow copy of a list.
2. When used on the left side of an assignment, it modifies the original list instead of creating a new one.

#### Why Use It? 🤔 <a href="#why-use-it" id="why-use-it"></a>

1. **In-place Modification**: Updates the list without creating a new object.
2. **Reference Preservation**: Maintains existing references to the list.
3. **Efficiency**: Often more performant than creating a new list, especially for large datasets.

#### Common Use Cases 🛠️ <a href="#common-use-cases" id="common-use-cases"></a>

1.  Clearing a list:

    `my_list[:] = []`
2.  Replacing list contents:

    `my_list[:] = [1, 2, 3, 4, 5]`
3.  Reversing a list:

    `my_list[:] = my_list[::-1]`

#### Real-World Example: `os.walk()` 🌳 <a href="#real-world-example-os.walk" id="real-world-example-os.walk"></a>

One practical application is with `os.walk()` for directory traversal. Here's an example:

```python
import os

def get_files_recursively(path, skip_dirs):
    files = []
    for root, dirs, filenames in os.walk(path):
        # Modify dirs in-place to control traversal
        dirs[:] = [d for d in dirs if d not in skip_dirs]
        files.extend(os.path.join(root, f) for f in filenames)
    return files

# Usage
skip_dirs = {'.git', 'node_modules'}
files = get_files_recursively('/path/to/directory', skip_dirs)
```

In this example, `dirs[:] = ...` modifies the list that `os.walk()` uses to determine which subdirectories to visit next. This efficiently prunes the search without breaking the `os.walk()` iterator.

#### When to Use 🎯 <a href="#when-to-use" id="when-to-use"></a>

* Working with functions that reference the original list
* Needing to update list contents while preserving existing references
* Optimising performance for in-place list modifications
