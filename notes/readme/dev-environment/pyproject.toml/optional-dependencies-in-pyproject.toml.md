---
icon: alt
---

# Optional Dependencies in pyproject.toml

{% hint style="info" %}
`[project.optional-dependencies]` in `pyproject.toml` lets you define groups of optional dependencies (like `dev`, `doc`, `test`) that are installed only when explicitly specified during `pip install`. This keeps default installs leaner and faster. Use `pip install my-project[group1,group2]` or `pip install -e .[group1]` for editable installs with optional dependencies.
{% endhint %}



`[project.optional-dependencies]` in `pyproject.toml` 📌

This section allows defining optional dependency groups, so they are not installed by default with `pip install <project_name>`. This keeps installs faster and environments smaller.

**Defining Dependency Groups:**

Inside `[project.optional-dependencies]`, you define named groups. Common ones:

* `dev`: Development tools (testing, linting, debugging). Example: `pytest`, `flake8`, `coverage`
* `doc`: Documentation tools. Example: `sphinx`, `sphinx-rtd-theme`
* `test`: Testing tools. If not present, `pytest` looks for and installs dependencies listed in `dev` group.
* Custom groups: Categorize dependencies for specific project features (e.g., `visualization`, `data-science`).

Example:

```yaml
[project.optional-dependencies]
dev = ["pytest", "flake8", "coverage"]
doc = ["sphinx", "sphinx-rtd-theme"]
visualization = ["matplotlib", "seaborn"]
```

**Installing Optional Dependencies:**

* `pip install <project_name>[<group_name>]`
* `pip install my-project[dev]` (installs `dev` dependencies)
* `pip install my-project[dev,visualization]` (installs both groups)

**Editable Installs with Optional Dependencies:**

* `pip install -e .[dev]` (editable install + `dev` dependencies)

**Benefits:**

* ✅ Faster installation: Only installs required dependencies by default.
* ✅ Smaller virtual environments: Avoids unnecessary packages.
* ✅ Clearer dependency management: Groups dependencies by purpose.
* ✅ Improved reproducibility: Explicitly define environments for tasks.

**Example Use Case:**

Imagine a project with heavy data science dependencies. You might define a `data-science` group. Developers who only work on the core application wouldn't need these, so they would install without that group. Data scientists working on analysis features could install with `pip install -e .[dev,data-science]` to get all the tools they need.
