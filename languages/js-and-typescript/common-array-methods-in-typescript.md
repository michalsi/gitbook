---
icon: objects-column
---

# Common Array Methods in TypeScript

**TLDR**: Common array methods: `map()` transforms elements, `filter()` selects elements, `reduce()` accumulates, `forEach()` iterates, `find()` gets first match.

#### 🗺️ map()

Transforms each element, returns new array

```typescript
let numbers = [1, 2, 3, 4];

// Double each number
let doubled = numbers.map(num => num * 2);
// Result: [2, 4, 6, 8]

// Convert to strings
let strings = numbers.map(num => `Number ${num}`);
// Result: ["Number 1", "Number 2", "Number 3", "Number 4"]
```

#### 🔍 filter()

Creates new array with elements that pass test

```typescript
let numbers = [1, 2, 3, 4, 5, 6];

// Get even numbers
let evenNums = numbers.filter(num => num % 2 === 0);
// Result: [2, 4, 6]

// Numbers greater than 3
let bigNums = numbers.filter(num => num > 3);
// Result: [4, 5, 6]
```

#### ➰ forEach()

Executes function on each element, no return

```typescript
let fruits = ['apple', 'banana', 'orange'];

fruits.forEach(fruit => {
    console.log(`I like ${fruit}`);
});
// Logs:
// "I like apple"
// "I like banana"
// "I like orange"
```

#### 🔎 find()

Returns first element that passes test

```typescript
let users = [
    {id: 1, name: 'John'},
    {id: 2, name: 'Jane'},
    {id: 3, name: 'Bob'}
];

let user = users.find(u => u.id === 2);
// Result: {id: 2, name: 'Jane'}
```

#### 📝 some() & every()

Check conditions across array

```typescript
let numbers = [1, 2, 3, 4, 5];

// Are any numbers greater than 4?
let hasOverFour = numbers.some(num => num > 4);
// Result: true

// Are all numbers less than 10?
let allUnderTen = numbers.every(num => num < 10);
// Result: true
```

#### 📊 reduce()

Accumulates values into single result

```typescript
let numbers = [1, 2, 3, 4];

// Simple sum
let sum = numbers.reduce((acc, num) => acc + num, 0);
// Result: 10

// More complex example - group by even/odd
let grouped = numbers.reduce((acc, num) => {
    let key = num % 2 === 0 ? 'even' : 'odd';
    acc[key].push(num);
    return acc;
}, { even: [], odd: [] });
// Result: { even: [2, 4], odd: [1, 3] }

// Create object from array
let users = ['John', 'Jane', 'Bob'];
let userObj = users.reduce((acc, name, index) => {
    acc[index] = name;
    return acc;
}, {});
// Result: { 0: 'John', 1: 'Jane', 2: 'Bob' }
```

#### 🔄 Chaining Methods

```typescript
let data = [1, 2, 3, 4, 5, 6];

let result = data
    .filter(num => num % 2 === 0)    // Get even numbers
    .map(num => num * 2)             // Double them
    .reduce((acc, num) => acc + num); // Sum them up

// Result: 24 (2+4+6 -> 4+8+12 -> 24)
```

#### ⚡ Performance Notes

* `map()`: Creates new array
* `filter()`: Creates new array
* `reduce()`: Single value result
* `forEach()`: No new array, can't break
* `find()`: Stops at first match

#### 🎯 When to Use What

* `map()`: Transform elements
* `filter()`: Select elements
* `reduce()`: Accumulate/combine
* `forEach()`: Side effects
* `find()`: Search for element
* `some()/every()`: Validate conditions
