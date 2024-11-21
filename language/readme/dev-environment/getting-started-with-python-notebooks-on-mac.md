---
icon: ballot-check
---

# Getting Started with Python Notebooks on Mac

{% hint style="success" %}
### TL;DR <a href="#tl-dr" id="tl-dr"></a>

* **Python Notebooks**: Interactive files that combine code, text, and visuals. Great for data science and research.
* **Set Up on Mac**:
  1. **Install Python**: `brew install python`
  2. **Create Virtual Environment**:
     * `python3 -m venv myenv`
     * `source myenv/bin/activate`
  3. **Install Jupyter**: `pip install jupyter`
  4. **Launch Notebook**: `jupyter notebook`
* **Outcome**: Get a flexible, isolated environment for coding and data analysis.
{% endhint %}

Python notebooks, also known as Jupyter Notebooks, are powerful tools that allow you to combine code, text, and visualisations in a single interactive document. They're widely used in data science, machine learning, research, and education due to their flexibility and ease of use. In this blog post, we'll explore what Python notebooks are and guide you through the process of installing and setting them up on a Mac using Homebrew and virtual environments.

### What Are Python Notebooks? <a href="#what-are-python-notebooks" id="what-are-python-notebooks"></a>

Python notebooks are files with a `.ipynb` extension. They allow you to:

* **Write and Execute Code:** Run Python code in separate cells, making it easy to test and debug.
* **Document Your Work:** Add explanations, equations (using LaTeX), and images using Markdown.
* **Visualize Data:** Create plots and charts directly within the notebook using popular libraries like Matplotlib and Seaborn.
* **Share and Collaborate:** Easily share your notebook with others for collaborative work or presentations.

### Installing Jupyter Notebooks on Mac <a href="#installing-jupyter-notebooks-on-mac" id="installing-jupyter-notebooks-on-mac"></a>

To get started with Jupyter Notebooks on your Mac, follow these simple steps. We'll use Homebrew to install Python and leverage virtual environments to manage dependencies.

#### Step 1: Install Python <a href="#step-1-install-python" id="step-1-install-python"></a>

First, ensure Python is installed on your machine. If not, use Homebrew to install it:

`brew install python`

#### Step 2: Set Up a Virtual Environment <a href="#step-2-set-up-a-virtual-environment" id="step-2-set-up-a-virtual-environment"></a>

Creating a virtual environment is a great way to manage project-specific dependencies without affecting your system Python installation.

1.  **Create a Virtual Environment:**

    Open Terminal and run:

    `python3 -m venv myenv`

    This command creates a new virtual environment named `myenv`.
2.  **Activate the Virtual Environment:**

    Activate the environment with:

    `source myenv/bin/activate`

    You’ll know it’s active when the terminal prompt is prefixed with `(myenv)`.

#### Step 3: Install Jupyter Notebook <a href="#step-3-install-jupyter-notebook" id="step-3-install-jupyter-notebook"></a>

With the virtual environment activated, install Jupyter using pip:

`pip install jupyter`

#### Step 4: Launch Jupyter Notebook <a href="#step-4-launch-jupyter-notebook" id="step-4-launch-jupyter-notebook"></a>

Once installed, you can start Jupyter Notebook by running:

`jupyter notebook`

This command will open Jupyter Notebook in your default web browser, ready for you to start creating and managing notebooks.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Setting up Jupyter Notebooks on your Mac is straightforward with Homebrew and virtual environments. This setup ensures a clean workspace for your data science and development projects, allowing you to focus on what's important—writing and executing your code, documenting your process, and visualising your results. Whether you're a student, researcher, or developer, Jupyter Notebooks provide a versatile platform for your interactive computing needs.
