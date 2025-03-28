---
icon: whale
---

# Docker Images vs. Containers

{% hint style="info" %}
* I**mages:** Immutable, read-only templates for creating containers.
* **Containers:** Mutable, running instances of images, encapsulating applications.
{% endhint %}

***

#### Introduction

Docker is a game-changer in the world of software development, allowing us to package applications in a consistent and portable manner. To make the most of Docker, it's crucial to understand the difference between **images** and **containers**. Let's break it down!

***

* **What Are They?**\
  Docker images are like blueprints for your applications, containing everything needed to run them: code, dependencies, environment variables, and configurations.
* **Key Characteristics:**
  * **Immutable:** Once an image is built, it cannot change.
  * **Portable:** Shareable across environments via Docker registries.
  * **Versioned:** Use tags like `myapp:1.0` to manage versions.
*   **Example:**

    ```plaintext
    FROM python:3.9-slim
    COPY . /app
    WORKDIR /app
    RUN pip install -r requirements.txt
    CMD ["python", "app.py"]
    ```

    This Dockerfile builds an image with Python 3.9, copies the app code, installs dependencies, and runs the application.

***

#### 🏗️ Docker Containers

* **What Are They?**\
  Containers are running instances of images, executing the application in an isolated environment.
* **Key Characteristics:**
  * **Ephemeral:** Easily start, stop, and remove containers.
  * **Isolated:** Each container has its own filesystem and network.
  * **Stateful:** Can persist data via volumes.
*   **Example:**

    ```bash
    docker run -d -p 5000:5000 myapp:1.0
    ```

    This command runs a container from the `myapp:1.0` image, mapping container port 5000 to host port 5000.

***

#### 🛠️ Practical Implementation

*   **Building an Image:**

    ```bash
    docker build -t myapp:1.0 .
    ```

    Use this command in the directory with your Dockerfile to build the image.
*   **Running a Container:**

    ```bash
    docker run --name myapp_container -d myapp:1.0
    ```

    Start a container named `myapp_container` from the image.
*   **Inspecting Containers:**

    ```bash
    docker ps
    ```

    List running containers to see their status and ports.

***

#### 📊 Visual Summary

```plaintext
+-------------------+
|  Docker Image     |
|-------------------|
|  Immutable        |
|  Portable         |
|  Versioned        |
+---------+---------+
          |
          v
+---------+---------+
| Docker Container  |
|-------------------|
|  Ephemeral        |
|  Isolated         |
|  Stateful         |
+-------------------+
```

***

#### 🌟 Conclusion

By understanding these core concepts of Docker, you'll be better equipped to leverage its power for developing and deploying applications efficiently. Remember, images are your immutable blueprints, while containers are your dynamic, running instances.
