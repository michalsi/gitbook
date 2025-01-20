---
icon: square-sliders-vertical
---

# pyproject.toml

The `pyproject.toml` file is a configuration file used in Python projects to define build system requirements and project metadata. Introduced in PEP 518 and expanded upon in subsequent PEPs, it aims to standardize how Python projects declare their build dependencies and configurations, thereby facilitating better interoperability between build tools.

### Key Components of `pyproject.toml` <a href="#key-components-of-pyproject.toml" id="key-components-of-pyproject.toml"></a>

**Build System Requirements:**

* **\[build-system] Table:** This section specifies the build system and the dependencies required to build the project. It typically includes the build backend and versions of any tools needed.
* **Example:**

```
[build-system]
requires = ["setuptools>=42", "wheel"]
build-backend = "setuptools.build_meta"
```

**Project Metadata:**

* **\[project] Table:** This is a more recent addition (PEP 621) that allows you to define metadata about the project, such as its name, version, authors, dependencies, and more.
* **Example:**

```
[project]
name = "my_package"
version = "0.1.0"
description = "A sample Python package"
authors = [
  { name="Jane Doe", email="jane@example.com" }
]
dependencies = [
  "requests>=2.25",
  "numpy>=1.19"
]
```

**Tool-Specific Configuration:**

* **\[tool] Table:** This section allows other tools to store their configuration settings. For example, `black`, a Python code formatter, can store its settings under `[tool.black]`.
* **Example:**

```
[tool.black]
line-length = 88
```

#### Benefits of `pyproject.toml` <a href="#benefits-of-pyproject.toml" id="benefits-of-pyproject.toml"></a>

* **Standardization:** It provides a standardized way to declare build dependencies, which helps reduce issues arising from mismatched environments and makes the build process more predictable.
* **Interoperability:** Facilitates better interoperability between different Python tools by providing a common configuration file format.
* **Simplicity and Clarity:** By consolidating various configurations into a single file, it simplifies project setup and maintenance.
* **Future-Proofing:** As the Python packaging ecosystem evolves, `pyproject.toml` serves as a flexible foundation that can accommodate future enhancements and changes.

#### Adoption and Usage <a href="#adoption-and-usage" id="adoption-and-usage"></a>

* **Supported Tools:** Many modern Python tools and build systems, such as `setuptools`, `poetry`, and `flit`, recognize and utilize `pyproject.toml`.
* **Legacy Projects:** While not all existing projects use `pyproject.toml`, it is increasingly recommended for new projects, especially those looking to leverage modern build tools and practices.
