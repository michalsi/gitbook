# Setting Up Virtual Environments with pipenv in VS Code on macOS 🤖

Managing Python projects with `pipenv` ensures a clean and isolated development environment. Here’s how to set it up with Visual Studio Code on macOS:

#### 1. **Install Libraries with `pipenv`**

First, install your libraries using `pipenv` to automatically update the `Pipfile`:

```bash
pipenv install <library_name>
```

This keeps your dependencies organized and version-locked in the `Pipfile` and `Pipfile.lock`.

#### 2. **Find Your Virtual Environment**

To locate the virtual environment created by `pipenv`, run:

```bash
pipenv --venv
```

This will return the path to the virtual environment, for example:

```plaintext
/Users/yourusername/.local/share/virtualenvs/YourProject-abc123
```

#### 3. **Open Your Project in VS Code**

Navigate to your project directory and open it in VS Code:

```bash
cd /path/to/your/project
code .
```

#### 4. **Select the Correct Python Interpreter**

1. Press **Cmd + Shift + P** to open the Command Palette.
2. Type `Python: Select Interpreter` and press Enter.
3. Select the interpreter corresponding to your virtual environment. It will look something like:

```plaintext
Python 3.x ('YourProject': pipenv) (/Users/yourusername/.local/share/virtualenvs/YourProject-abc123/bin/python)
```

If it doesn’t appear, manually enter the path to the Python binary in your virtual environment:

```plaintext
/Users/yourusername/.local/share/virtualenvs/YourProject-abc123/bin/python
```

#### 5. **Verify Configuration**

Open the integrated terminal in VS Code (**Ctrl + `** or **Cmd +`** on macOS) and confirm the active Python environment:

```bash
python --version
```

Test your installed libraries:

```bash
python -c "import requests; print(requests.__version__)"
```

#### 6. **Restart VS Code**

Restart VS Code after selecting the interpreter to ensure all changes take effect.

#### 7. **Optional Debugging Configuration**

If you’re using a `launch.json` file for debugging, update it to use the correct Python path:

```json
{
    "configurations": [
        {
            "type": "python",
            "request": "launch",
            "name": "Python: Current File",
            "program": "${file}",
            "pythonPath": "/Users/yourusername/.local/share/virtualenvs/YourProject-abc123/bin/python"
        }
    ]
}
```

***

With this setup, you can confidently manage Python virtual environments in VS Code on macOS. No more library-not-found issues! 🌐
