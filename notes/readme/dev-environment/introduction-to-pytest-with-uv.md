---
icon: tally-4
---

# Introduction to Pytest with UV

`pytest` is a powerful testing framework for Python that simplifies writing and running tests. It’s known for its easy-to-use syntax and robust features, making it a popular choice for both small and large projects. In this guide, we'll cover how to configure and run `pytest` using `uv`, a fast Python package and project manager, and discuss output capturing.

### Setting Up Pytest <a href="#setting-up-pytest" id="setting-up-pytest"></a>

#### Installation <a href="#installation" id="installation"></a>

To install `pytest`, you can use `uv`, assuming it's already installed. If not, install `uv` via `pip` first:

`pip install uv`

Then, install `pytest` using `uv`:

`uv install pytest`

This command will add `pytest` to your project's dependencies, managing versioning and installation efficiently.

#### Basic Configuration <a href="#basic-configuration" id="basic-configuration"></a>

`pytest` can be configured using a `pyproject.toml` file in your project’s root directory. Here's a simple example to get you started:

```
[tool.pytest.ini_options]
addopts = "--maxfail=3 --disable-warnings"
testpaths = ["tests"]
```

This configuration specifies:

* `addopts`: Additional options passed to `pytest`, such as stopping after 3 failures and disabling warnings.
* `testpaths`: Directories to search for test files.

### Running Pytest with UV <a href="#running-pytest-with-uv" id="running-pytest-with-uv"></a>

To run your tests using `uv`, you can execute the following command:

`uv run pytest`

This command will discover and execute all test files and functions following the naming conventions (e.g., files starting with `test_` or ending with `_test`).

### Output Capturing in Pytest <a href="#output-capturing-in-pytest" id="output-capturing-in-pytest"></a>

By default, `pytest` captures all output (including `print` statements) to keep test output clean. This captured output is only shown if a test fails. However, you can modify this behavior:

#### Viewing Print Statements <a href="#viewing-print-statements" id="viewing-print-statements"></a>

To see print statements during test execution, use the `-s` option:

`uv run pytest -s`

This command disables output capturing, allowing all terminal output to be displayed.

#### Using Logging <a href="#using-logging" id="using-logging"></a>

Instead of `print`, consider using Python's built-in `logging` module for more control over output. Configure logging to display desired log levels:

`uv run pytest --log-cli-level=INFO`

In your test script:

```python
import logging
logger = logging.getLogger(__name__)
def test_logging():
    logger.info("Info log message")
    assert True
```

This approach integrates well with `pytest`, showing logs alongside test results.

### Example Test File <a href="#example-test-file" id="example-test-file"></a>

Here's a simple example test file to illustrate the basic concepts:

```python
# test_example.py

def test_addition():
    assert 1 + 1 == 2
    
def test_subtraction():
    assert 2 - 1 == 1
```

Place this file in your `tests` directory, and run it using:

`uv run pytest`
