---
icon: pallet-box
---

# argparse.Namespace

{% hint style="info" %}
`argparse.Namespace` is a simple object used to store command-line arguments as attributes. It's useful for simulating and testing command-line inputs in Python scripts, especially for unit testing.
{% endhint %}

### What is `argparse.Namespace`? <a href="#what-is-argparse.namespace" id="what-is-argparse.namespace"></a>

* **Purpose:** A part of the `argparse` module in Python, designed to encapsulate command-line arguments as attributes for easy access.
* **Structure:** Acts like an object where each command-line argument is stored as an attribute. For example, `args.verbose` for a `--verbose` flag.

### Creating a `Namespace` Object <a href="#creating-a-namespace-object" id="creating-a-namespace-object"></a>

* **Manual Creation:** You can create a `Namespace` object by passing keyword arguments:

```python
import argparse
args = argparse.Namespace(verbose=True, file_path="example.txt")
```

* Here, `verbose` and `file_path` become attributes of the `args` object.

### Usage in Testing <a href="#usage-in-testing" id="usage-in-testing"></a>

* **Mocking Arguments:** Simulate command-line arguments without needing to parse real input.
* **Example:**

```python
def process_file(args):
    if args.verbose:
        print(f"Processing file: {args.file_path}")

# Mock arguments for testing
args = argparse.Namespace(verbose=True, file_path="example.txt")
process_file(args)
```

### Benefits of Using `argparse.Namespace` <a href="#benefits-of-using-argparse.namespace" id="benefits-of-using-argparse.namespace"></a>

* **👍 Flexibility:** Quickly create various argument configurations by changing the attributes.
* **👨‍🔬 Simplicity:** Mirrors the structure of parsed arguments, making it intuitive and easy to understand.
* **🔍 Isolation:** Focuses testing on function logic rather than command-line parsing logic, enhancing test clarity.

### When to Use `argparse.Namespace` <a href="#when-to-use-argparse.namespace" id="when-to-use-argparse.namespace"></a>

* **Unit Testing:** For functions that rely on parsed command-line arguments.
* **Mocking:** In scenarios where you want to replicate the behaviour of command-line arguments without executing the parsing logic.
