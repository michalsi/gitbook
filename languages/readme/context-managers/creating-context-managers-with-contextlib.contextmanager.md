---
description: >-
  The `contextlib.contextmanager` decorator in Python allows you to create
  context managers using generator functions. This approach provides a concise
  and readable way to manage resources,
icon: books
---

# Creating Context Managers with \`contextlib.contextmanager\`

### Understanding `contextlib.contextmanager`

The `contextlib` module in Python provides tools for creating and working with context managers. One of its most powerful features is the `contextmanager` decorator, which simplifies the creation of context managers by using generator functions.

#### Structure of a Context Manager with `contextlib.contextmanager`

Here's how you typically structure a context manager using the `contextmanager` decorator:

```python
from contextlib import contextmanager

@contextmanager
def my_context():
    # Setup code (executed before the block)
    print("Entering context")
    resource = "Resource"
    try:
        yield resource  # The block using the context manager is executed here
    finally:
        # Teardown code (executed after the block, even if exceptions occur)
        print("Exiting context")
```

#### How It Works ⚙️ <a href="#how-it-works" id="how-it-works"></a>

1. **Setup Code**:
   * This is executed before the block under the `with` statement.
   * Typically involves acquiring resources like opening files or connecting to databases.
2. `yield` Statement:
   * Temporarily suspends the function’s execution, passing control to the `with` block.
   * The value yielded is assigned to the variable after `as` in the `with` statement.
3. **Block Execution**:
   * The code block under the `with` statement runs between the `yield` and the `finally` block.
4. **Teardown Code**:
   * Runs after the `with` block completes, even if exceptions occur, ensuring resources are released properly.

#### Handling Exceptions <a href="#handling-exceptions" id="handling-exceptions"></a>

* Exceptions in the `with` block are propagated back to the `yield` expression.
* You can add `except` clauses around `yield` to handle specific exceptions if needed.

### Example with Exception Handling

```python
from contextlib import contextmanager

@contextmanager
def my_context():
    print("Entering context")
    resource = "Resource"
    try:
        yield resource
    except Exception as e:
        print(f"An exception occurred: {e}")
    finally:
        print("Exiting context")
# Usage
with my_context() as res:
    print(f"Using {res}")
    # the line below used to see exception handling in action
    raise ValueError("An error occurred")
```

#### Benefits 🌟 <a href="#benefits" id="benefits"></a>

* **Conciseness**: Encapsulates both setup and teardown logic in a single, small function.
* **Readability**: Clearly separates resource acquisition and release, improving code clarity.
* **Flexibility**: Easily handle exceptions and manage resources effectively.
