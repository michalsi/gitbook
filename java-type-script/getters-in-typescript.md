---
icon: get-pocket
---

# Getters in TypeScript

#### What Are Getters?

A getter in TypeScript is a special type of method that is used to access a property of a class. Unlike regular methods, getters are accessed like properties but can contain logic to compute a value or manage the retrieval of a property dynamically.

#### Syntax

Here's the basic syntax for defining a getter in TypeScript within a class:

```typescript
class MyClass {
    private _property: string = 'default value';

    get property(): string {
        console.log('Getter called');
        return this._property;
    }
}

const instance = new MyClass();
console.log(instance.property); // Outputs: 'Getter called' followed by 'default value'
```

#### Key Characteristics

* **Access Like Properties**: Getters allow you to access methods like properties. In the example above, `instance.property` calls the `get property()` method without parentheses.
* **Read-Only by Default**: Getters do not provide a way to set the value of the property. If you need to set a property, you would typically also define a setter.
* **Encapsulation**: They can be used to encapsulate logic for retrieving a property, such as lazy loading, validation, or formatting.
* **Computed Properties**: Useful for properties that need to be computed dynamically based on other instance variables.

#### Benefits of Using Getters

1. **Lazy Initialization**:
   * Getters can delay the creation and initialization of an object until it is actually needed, which can save resources and improve performance.
2. **Encapsulation and Abstraction**:
   * They hide the internal representation of properties, allowing you to change the internal implementation without affecting external code that uses the class.
3. **Validation and Control**:
   * Use getters to control access to a property, for example, you might check a condition before allowing a property to be accessed.

#### Practical Example: Lazy Initialization in Page Objects

In the context of your Playwright test setup, using getters for page objects can optimize resource usage by only instantiating page objects when they are first accessed.

```typescript
class PageObjects {
    private _assetPage?: AssetPage;

    constructor(private page: Page) {}

    get assetPage(): AssetPage {
        if (!this._assetPage) {
            console.log('Creating AssetPage instance');
            this._assetPage = new AssetPage(this.page);
        }
        return this._assetPage;
    }

    // Similar getters for other page objects
}

// Usage
const pageObjects = new PageObjects(page);
const assetPage = pageObjects.assetPage; // AssetPage is instantiated here
```

#### Considerations

* **Thread-Safety**: In environments where code execution is concurrent, ensure that the lazy initialization logic in getters is thread-safe.
* **Performance**: While getters can improve performance by delaying initialization, they can also introduce overhead if the logic within a getter is complex or called frequently.
* **Readability**: Overusing getters for non-trivial logic can make code harder to understand. It's essential to maintain a balance and use them judiciously.

#### Conclusion

Getters in TypeScript provide a clean and efficient way to manage property access, especially when you need to perform additional logic or control over how properties are accessed. They are particularly useful for implementing lazy loading patterns, which can be highly beneficial in optimizing resource use in your tests.
