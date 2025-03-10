---
icon: comment-lines
---

# Understanding Conventional Commits

Conventional Commits is a specification for structuring commit messages. It provides a standardized way of describing changes, making it easier to read and automate processes like changelog generation. Key commit types include `feat`, `fix`, `docs`, among others. Using these conventions helps in improving collaboration and automation in software projects. 💾

***

### What are Conventional Commits?

Conventional Commits is a convention for writing commit messages that provide a consistent way to communicate the nature of changes in a codebase. This helps both humans and machines understand what changes have been made.

#### Basic Structure:

* **Type**: The kind of change (`feat`, `fix`, etc.)
* **Description**: Briefly explain what has been changed.

```plaintext
<type>: <description>
```

#### Example:

```plaintext
feat: add mermaid diagram generation workflow
```

* **Type**: `feat` (indicating a new feature)
* **Description**: `add mermaid diagram generation workflow`

### 🔍 Common Types of Conventional Commits:

* **feat**: A new feature
  * Example: `git commit -m "feat: add user authentication"`
* **fix**: A bug fix
  * Example: `git commit -m "fix: correct calculation of trading bot profit"`
* **docs**: Documentation only changes
  * Example: `git commit -m "docs: update README with setup instructions"`
* **style**: Changes that don't affect the meaning of the code (e.g., formatting)
  * Example: `git commit -m "style: reformat code"`
* **refactor**: Code changes that neither fix a bug nor add a feature
  * Example: `git commit -m "refactor: optimize algorithm for performance"`
* **test**: Adding missing tests or correcting existing tests
  * Example: `git commit -m "test: add unit tests for login component"`
* **chore**: Changes to the build process or auxiliary tools
  * Example: `git commit -m "chore: update npm dependencies"`
* **perf**: A code change that improves performance
  * Example: `git commit -m "perf: improve database query speed"`
* **ci**: Changes to CI configuration files and scripts
  * Example: `git commit -m "ci: add automated testing workflow"`

### 🛠️ Benefits of Using Conventional Commits:

1. **Automated Changelog Generation**: Tools can automatically create changelogs from commit messages.
2. **Semantic Versioning**: Helps in determining version bumps (major, minor, patch).
3. **Clear Communication**: Team members can quickly understand the nature of changes.
4. **Automation Triggers**: Can trigger builds, tests, or deployments based on commit messages.
5. **Facilitates Contribution**: Makes it easier for others to understand and contribute to the project.

### 📋 Example of a Detailed Commit Message:

```plaintext
git commit -m "feat: add mermaid diagram generation workflow
- Add GitHub Action to automatically generate SVG from Mermaid
- Create directory structure for architecture diagrams
- Add initial component architecture diagram
BREAKING CHANGE: requires Node.js 16+ for Mermaid CLI"
```

#### Key Points:

* **Scope**: Optional, can specify a part of the app (`feat(auth): ...`).
* **Body**: Detailed explanation of changes.
* **Footer**: Includes breaking changes or important notes.

***

### Conclusion

Using Conventional Commits can greatly enhance the clarity and efficiency of your development process. It ensures that everyone on your team, along with various tools and processes, understands the changes being made to the codebase. Keeping your commit messages clear and structured helps maintain a healthy and productive development environment. Happy coding! 🚀
