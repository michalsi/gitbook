---
icon: box-open
---

# pipx: Python Application Manager

{% hint style="info" %}
**pipx** installs and runs Python applications in isolated environments, allowing global access without conflicts.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

📊 How pipx works:

```
 [Your Computer]
|
|-- [System Python]
|
|-- [pipx]
    |
    |-- [Tool A]
    |   |-- [Isolated Environment A]
    |       |-- [Tool A's Python]
    |       |-- [Tool A's Dependencies]
    |
    |-- [Tool B]
    |   |-- [Isolated Environment B]
    |       |-- [Tool B's Python]
    |       |-- [Tool B's Dependencies]
```



### 🔧 Basic Usage:

* Install: pipx install package\_name
* Run: pipx run package\_name
* Uninstall: pipx uninstall package\_name
* List: pipx list
* Upgrade: pipx upgrade package\_name

### 💡 Use Cases:

* Installing CLI tools (e.g., black, flake8, poetry)
* Running one-off scripts
* Testing different versions of tools

### 🔄 Comparison:

* pip: Installs globally or in current venv
* Poetry/Conda: Project-level dependency management
* pipx: Standalone tool installation and execution

### 🔗 Integration:

* Use pipx to install project management tools (e.g., poetry)
* Install development tools (black, flake8, mypy) separately

### ⚠️ Considerations:

* Requires Python 3.6+
* Best for command-line applications
* Not a replacement for project-specific virtual environments
