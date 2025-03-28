---
icon: github
---

# Caching Virtual Environments in GitHub Actions

{% hint style="info" %}
&#x20;Use `actions/cache` to save and restore your `.venv` between workflow runs. A good `key` is crucial for correct cache invalidation. Use `hashFiles` on your `pyproject.toml` to ensure the cache is updated when dependencies change. `restore-keys` provide fallback options.
{% endhint %}

#### Purpose <a href="#purpose" id="purpose"></a>

Caching virtual environments significantly speeds up GitHub Actions workflows by reusing previously built environments instead of recreating them each time. This saves time and resources, especially when dealing with large projects or frequent workflow runs.

#### How It Works <a href="#how-it-works" id="how-it-works"></a>

The `actions/cache` action is used to store and restore the `.venv` directory. The `key` input determines whether a cached version exists and should be used. `restore-keys` provide fallback options if no exact key match is found.

#### Key Elements <a href="#key-elements" id="key-elements"></a>

* `path:`: Specifies the directory to cache (usually `.venv`).
* `key:`: The primary identifier for the cache. A good key should incorporate:
  * **OS:** `${{ runner.os }}` (virtual environments are OS-dependent).
  * **Dependency Definition:** `${{ hashFiles('**/pyproject.toml') }}` (ensures cache invalidation when dependencies change).
  * Example: `${{ runner.os }}-python-${{ hashFiles('**/pyproject.toml') }}`
* `restore-keys:`: Fallback keys to use if the primary key doesn't match. These should be less specific than the `key`.
  * Example: `${{ runner.os }}-python-` (matches any cache for the same OS).

#### Example <a href="#example" id="example"></a>

```yaml
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: .venv
    key: ${{ runner.os }}-python-${{ hashFiles('**/pyproject.toml') }}
    restore-keys: |
      ${{ runner.os }}-python-
```

#### Best Practices <a href="#best-practices" id="best-practices"></a>

* **Pin Dependency Versions:** Specify precise versions in `pyproject.toml` for reproducible builds (e.g., `"requests==2.28.1"`). This makes caching more effective.
* **Use** `[dev]` Dependencies: Group development dependencies for easier management and installation. Install with `pip install -e .[dev]`.
* **Consider Caching Other Dependencies:** If you have other large, unchanging dependencies, consider caching them separately to further optimize workflow speed.

#### Why `hashFiles` is Important 🤔 <a href="#why-hashfiles-is-important" id="why-hashfiles-is-important"></a>

`hashFiles` calculates a hash based on the contents of the specified files. By using it on `pyproject.toml`, the cache key changes whenever your project's dependencies change (because `pyproject.toml` is where you define them). This ensures that the cache is automatically invalidated and rebuilt when necessary, preventing issues caused by outdated dependencies. This is crucial for reliable and reproducible builds. ✨

#### &#x20; <a href="#example" id="example"></a>
