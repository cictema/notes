## Destructuring Arrays
---
### Basic Destructuring
```js
const arr = [1,2,3]
const [x,y,z] = arr
//x = 1
//y = 2
//z = 3

const arr = [1,2,3]
const [x] = arr
//x = 1
```

### Nested Destructuring
```js
const arr = [1,2,[3,4]]]
const [x,y,[z,a]] = arr
```

### Switch Variables
```js
const arr = [firstname, secondname]
[secondname, firstname] = arr

```
## Destructuring Objects
---
### Basic Destructuring
```js

//Same Name
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

// Destructuring the object
const { name, age, city } = person;

console.log(name); // Output: John
console.log(age);  // Output: 30
console.log(city); // Output: New York

//Different Name
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

// Destructuring the object with different variable names
const { name: firstName, age: personAge, city: hometown } = person;

console.log(firstName);  // Output: John
console.log(personAge);  // Output: 30
console.log(hometown);   // Output: New York


//Nested
const user = {
  id: 1,
  profile: {
    firstName: 'Jane',
    lastName: 'Doe'
  }
};

// Destructuring nested objects
const { profile: { firstName, lastName } } = user;

console.log(firstName); // Output: Jane
console.log(lastName);  // Output: Doe

```

### Object.values
```js
let languages = { por: "Portuguese", spa: "Spanish", eng: "English" };

// Get all values from the object
let values = Object.values(languages);

console.log(values); 
// Output: ["Portuguese", "Spanish", "English"]

```

### Object.keys
```js
let languages = { por: "Portuguese", spa: "Spanish", eng: "English" };

// Get all key-value pairs from the object
let entries = Object.keys(languages);

console.log(entries); 
// Output: ["por", "spa", "eng"]
```

### Object.entries
```js
let languages = { por: "Portuguese", spa: "Spanish", eng: "English" };

// Get all key-value pairs from the object
let entries = Object.entries(languages);

console.log(entries); 
// Output: [["por", "Portuguese"], ["spa", "Spanish"], ["eng", "English"]]

```
### Iterating over Entries
```js
for (const [key, value] of Object.entries(languages)) {
  console.log(`${key}: ${value}`);
}

// Output:
// por: Portuguese
// spa: Spanish
// eng: English

```

##  Spread Operator (...)
---
```js
const person = {
  name: 'John',
  age: 30,
  city: 'New York',
  occupation: 'Developer',
  hobby: 'Cycling'
};

// Destructuring 'name' and 'city', and collecting the rest
const { name, city, ...otherInfo } = person;

console.log(name);      // Output: John
console.log(city);      // Output: New York
console.log(otherInfo); // Output: { age: 30, occupation: 'Developer', hobby: 'Cycling' }

```

##  Rest Pattern and Parameters
---
When you assign values (on the left side for the equality sign, or function parameters).
```js
const { a, ...rest } = { a: 1, b: 2, c: 3 };
console.log(rest);  // Output: { b: 2, c: 3 }



function combine(first, ...rest) {  // Rest operator in parameters (gathers)
  const combined = [first, ...rest];  // Spread operator in array literal (spreads)
  return combined;
}
```


## Rest Vs Spread
---
### Key Differences Between Rest and Spread:

| Feature             | Rest Operator (`...`)                                                | Spread Operator (`...`)                                                       |     |
| ------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------------------- | --- |
| **Purpose**         | Collects multiple elements into a single variable (array or object). | Spreads elements from an array, object, or iterable into individual elements. |     |
| **Common Use Case** | Used in destructuring (arrays or objects) and function parameters.   | Used to copy/expand arrays or objects, and to pass arguments to functions.    |     |
| **Data Handling**   | Collects remaining elements or properties.                           | Expands elements into individual elements/properties.                         |     |
| **Example**         | `function sum(...args)` (collects arguments)                         | `console.log(...arr)` (spreads array into arguments)                          |     |

### Key Contexts for Differentiation:
- **Rest Operator**:
    - **Function Parameters**: `function(...args) { }`
    - **Destructuring**: `const [first, ...rest] = arr` or `const {a, ...rest} = obj`
- **Spread Operator**:
    - **Array Literals**: `[...arr]`
    - **Function Arguments**: `func(...args)`
    - **Object Literals**: `{ ...obj }`


### Rest Example:
```js
const { a, ...rest } = { a: 1, b: 2, c: 3 };
console.log(rest);  // Output: { b: 2, c: 3 }

```

### Spread Example:
```js
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];
console.log(newArr);  // Output: [1, 2, 3, 4, 5]
```

### Example: Using Rest and Spread Together
```js
function combine(first, ...rest) {  // Rest operator in parameters (gathers)
  const combined = [first, ...rest];  // Spread operator in array literal (spreads)
  return combined;
}

console.log(combine(1, 2, 3, 4));  // Output: [1, 2, 3, 4]

```
## Short Circuiting (&& and ||)
---
**Short-circuiting** in JavaScript refers to the behavior of logical operators (`&&` and `||`) that stop evaluating expressions as soon as the result is determined. This means they "short-circuit" the evaluation if the outcome can be inferred before checking all expressions.

### Short-circuiting with `||` (Logical OR)
The `||` operator evaluates expressions from left to right and **stops** when it encounters a **truthy** value, returning that value. If all values are falsy, it returns the last value.
```js
const a = false;
const b = true;
const result = a || b || "This won't be evaluated";

console.log(result);  // Output: true

```

### Short-circuiting with `&&` (Logical AND)

The `&&` operator evaluates expressions from left to right and **stops** when it encounters a **falsy** value, returning that falsy value. If all values are truthy, it returns the last value.
```js
const a = true;
const b = false;
const result = a && b && "This won't be evaluated";

console.log(result);  // Output: false
```

## Nullish Coalescing Operator
---
The ?? operator is called the Nullish Coalescing Operator in JavaScript. It was introduced in ES2020 and is used to provide a default value when dealing with null or undefined. It is similar to the || operator but with a key difference: ?? only checks for null or undefined, whereas || considers all falsy values.

```js
let result = value1 ?? value2;
//`value1`: The first expression.
//`value2`: The default value to be returned if `value1` is `null` or `undefined`.
```

### Short-Circuiting with `??`

- If `value1` is **not** `null` or `undefined`, it returns `value1`.
- If `value1` **is** `null` or `undefined`, it returns `value2`.

```js
let username = null;
let defaultName = "Guest";

let displayName = username ?? defaultName;
console.log(displayName);  // Output: Guest

```

##### Explanation:
- With `||`, `0` is considered falsy, so `result1` becomes `5`.
- With `??`, `0` is not `null` or `undefined`, so `result2` remains `0`.

### Summary:

- **`??` (Nullish Coalescing Operator)** only considers `null` or `undefined` as invalid and returns the second value if the first is `null` or `undefined`.
- **`||` (Logical OR)** considers all **falsy** values (`false`, `0`, `""`, `NaN`, `null`, `undefined`) and returns the second value if the first is falsy.

### Comparison with `||` (Logical OR):

The key difference between `??` and `||` is that `||` checks for **any falsy value** (`0`, `false`, `NaN`, `""`, `null`, `undefined`), whereas `??` only checks for **`null` or `undefined`**.

```js
let num1 = 0;
let result1 = num1 || 5;
let result2 = num1 ?? 5;

console.log(result1);  // Output: 5 (because 0 is falsy for `||`)
console.log(result2);  // Output: 0 (because 0 is not null or undefined for `??`)

```
## Logical Assignment Operators
---


## Looping Arrays
---
### for-of loop

The **`for...of` loop** in JavaScript is used to iterate over **iterable objects** like arrays, strings, sets, maps, and more. It allows you to loop through the values of an iterable rather than the properties (as in `for...in`).

```js
for (const element of iterable) {
    // code to be executed
}
```

```JS
const fruits = ['apple', 'banana', 'cherry'];

for (const fruit of fruits) {
    console.log(fruit);
}

```

```JS
const text = 'hello';

for (const char of text) {
    console.log(char);
}

```

### **Comparison: `for...of` vs `for...in`**:

- **`for...of`**: Iterates over the **values** of an iterable (like an array, string, set, etc.).
- **`for...in`**: Iterates over the **keys/indexes** of an object or array.

```js
const arr = ['a', 'b', 'c'];

// for...in iterates over the indices (keys)
for (const index in arr) {
    console.log(index);  // Output: 0, 1, 2 (indices of the array)
}

// for...of iterates over the values
for (const value of arr) {
    console.log(value);  // Output: 'a', 'b', 'c' (values of the array)
}

```

### Use Cases for `for...of`:

- **Arrays**: Iterate over array elements.
- **Strings**: Iterate over characters in a string.
- **Maps and Sets**: Iterate over values in a `Map` or `Set`.
- **NodeLists**: Iterate over DOM elements from `querySelectorAll`.

### Example: Iterating Over a Map:
```js
const userMap = new Map();
userMap.set('name', 'John');
userMap.set('age', 30);

for (const [key, value] of userMap) {
    console.log(`${key}: ${value}`);
}

```

## Enhanced Object Literals
---
Enhanced Object Literals in JavaScript, introduced in ES6 (ES2015), provide a more concise and readable way to define object properties, methods, and behaviors. They simplify object creation by reducing boilerplate code, making it easier to manage dynamic properties and methods.

Here are the main features of Enhanced Object Literals:

### 1. **Shorthand Property Names**

Instead of explicitly assigning the value of a variable to an object property with the same name, you can use a shorthand syntax.

#### Example:
```js
const name = 'John';
const age = 30;

// Before ES6
const person1 = {
    name: name,
    age: age
};

// Using enhanced object literals (shorthand)
const person2 = {
    name,
    age
};

console.log(person2);  // Output: { name: 'John', age: 30 }

```

In `person2`, we omit the redundancy of writing `name: name` and `age: age`. JavaScript automatically infers the property name from the variable name.

### 2. **Shorthand Method Definitions**

You can define methods within an object more concisely without the `function` keyword.

Example:
```js
const person = {
    name: 'John',
    greet() {
        return `Hello, my name is ${this.name}`;
    }
};

console.log(person.greet());  // Output: Hello, my name is John

```

Instead of writing `greet: function() {}`, you can simply define the method using shorthand: `greet() {}`.

### 3. **Computed Property Names**

You can dynamically define property names in an object using expressions enclosed in square brackets (`[]`).

```js
const property = 'age';

const person = {
    name: 'John',
    [property]: 30  // Computed property name
};

console.log(person.age);  // Output: 30

```

The `property` variable holds the value `'age'`, and `[property]` dynamically creates the property `age` on the `person` object.

### 4. **Concise Object Assignment from Functions**

You can create and return objects with concise syntax when passing variables or values to functions.

```js
function createPerson(name, age) {
    return {
        name,
        age
    };
}

const person = createPerson('John', 30);
console.log(person);  // Output: { name: 'John', age: 30 }
```

### 5. **Merging Objects with Spread Syntax**

Although not strictly part of enhanced object literals, you can use the **spread operator (`...`)** in combination with these features to easily clone or merge objects.

```js
const person = { name: 'John', age: 30 };
const job = { title: 'Developer' };

const personWithJob = { ...person, ...job };
console.log(personWithJob);  // Output: { name: 'John', age: 30, title: 'Developer' }

```
The spread operator allows merging `person` and `job` into a new object.

### 6. **Default Function Parameters in Methods**

Enhanced object literals also work seamlessly with default parameters in methods.

#### Example:
```js
const person = {
    name: 'John',
    greet(message = 'Hello') {
        return `${message}, my name is ${this.name}`;
    }
};

console.log(person.greet());  // Output: Hello, my name is John
console.log(person.greet('Hi'));  // Output: Hi, my name is John

```

### Putting It All Together:
```js
const property = 'hobby';

const person = {
    name: 'John',
    age: 30,
    [property]: 'Cycling',  // Computed property

    // Shorthand method
    greet() {
        console.log(`Hi, I'm ${this.name}, and I enjoy ${this.hobby}.`);
    }
};

person.greet();  // Output: Hi, I'm John, and I enjoy Cycling.

```

## Optional Chaining
---

**Optional chaining (`?.`)** is a feature introduced in ES2020 that allows you to safely access deeply nested properties of an object without having to manually check each level for `null` or `undefined`. If any part of the chain is `null` or `undefined`, the expression short-circuits and returns `undefined` instead of throwing an error.

```js
object?.property
object?.[expression]
object?.method?.()
```

Note: it's always `?.`, not `?`

## Sets
---
In JavaScript, a **`Set`** is a built-in object that allows you to store **unique values** of any type, whether primitive values or object references. A `Set` is similar to an array, but it **automatically removes duplicate values**, ensuring that every value is unique.

```js
// Creating an empty set
const mySet = new Set();

// Creating a set with initial values
const numbers = new Set([1, 2, 3, 3, 4]);
console.log(numbers);  // Output: Set(4) { 1, 2, 3, 4 } (duplicate 3 is ignored)

```
### **Common Set Methods and Properties**:

1. **`add()`**: Adds a new element to the set.
2. **`delete()`**: Removes an element from the set.
3. **`has()`**: Checks if a value exists in the set.
4. **`clear()`**: Removes all elements from the set.
5. **`size`**: Returns the number of elements in the set.
## Maps
---

In JavaScript, a **`Map`** is a collection of **key-value pairs** where both keys and values can be of any type (including objects and functions). Unlike plain objects, which only support strings and symbols as keys, a `Map` allows you to use any type of value (primitive or object) as a key, and it maintains the insertion order of the entries.


```js
// Creating a map with key-value pairs
const map = new Map([
    ['name', 'John'],
    ['age', 30],
    ['job', 'Developer']
]);

console.log(map);  // Output: Map(3) { 'name' => 'John', 'age' => 30, 'job' => 'Developer' }

```

### **Map Methods**:

Maps provide several useful methods for adding, removing, and working with key-value pairs.

**`set(key, value)`**: Adds or updates a key-value pair in the map.
```js
const map = new Map();
map.set('name', 'Alice');
map.set('age', 25);

console.log(map);  // Output: Map(2) { 'name' => 'Alice', 'age' => 25 }
```

**`get(key)`**: Retrieves the value associated with the key.
```js
console.log(map.get('name'));  // Output: Alice
console.log(map.get('age'));   // Output: 25
```

**`has(key)`**: Checks if a key exists in the map.
```js
console.log(map.has('name'));  // Output: true
console.log(map.has('salary')); // Output: false
```

**`delete(key)`**: Removes a key-value pair from the map.
```js
map.delete('age');
console.log(map);  // Output: Map(1) { 'name' => 'Alice' }
```

**`clear()`**: Removes all key-value pairs from the map.
```js
map.clear();
console.log(map);  // Output: Map(0) {}
```

**`size`**: Returns the number of key-value pairs in the map.
```js
const map = new Map([['name', 'Alice'], ['age', 25]]);
console.log(map.size);  // Output: 2
```


### Convert Map to Array
#### Convert Map to Array of Key-Value Pairs

The `Map`'s `entries()` method returns an iterable of key-value pairs, which you can convert into an array using the spread operator (`...`) or `Array.from()`.
```js
const map = new Map([
    ['name', 'Alice'],
    ['age', 25],
    ['job', 'Developer']
]);

// Convert map to an array of key-value pairs
const mapToArray = [...map];
console.log(mapToArray);
// Output: [['name', 'Alice'], ['age', 25], ['job', 'Developer']]

```

Alternatively, you can use `Array.from()`:
```js
const mapToArray = Array.from(map);
console.log(mapToArray);
// Output: [['name', 'Alice'], ['age', 25], ['job', 'Developer']]
```

#### Convert Map Keys to an Array
```js
const keysArray = [...map.keys()];
console.log(keysArray);
// Output: ['name', 'age', 'job']
```

```js
const keysArray = Array.from(map.keys());
console.log(keysArray);
// Output: ['name', 'age', 'job']
```

#### Convert Map Values to an Array
```js
const valuesArray = [...map.values()];
console.log(valuesArray);
// Output: ['Alice', 25, 'Developer']



const valuesArray = Array.from(map.values());
console.log(valuesArray);
// Output: ['Alice', 25, 'Developer']

```

#### Using `forEach()` to Create a Custom Array
```js
const customArray = [];
map.forEach((value, key) => {
    customArray.push({ key, value });
});

console.log(customArray);
// Output: [{ key: 'name', value: 'Alice' }, { key: 'age', value: 25 }, { key: 'job', value: 'Developer' }]
```


## Working with Strings
---

### **Key Methods**:

### Basic Manipulation

`toUpperCase()`, `toLowerCase()`, `charAt()`, `concat()`

### Substring Extraction

`slice()`, `substring()`, `substr()`
```js
const text = 'JavaScript is fun';
console.log(text.slice(0, 10));  // Output: JavaScript
console.log(text.slice(-3));     // Output: fun (negative index counts from the end)
```

### Searching

`indexOf()`, `includes()`, `startsWith()`, `endsWith()`

### Replacement

`replace()`

### Splitting

`split()`
```js
const text = 'apple, banana, cherry';
const fruits = text.split(', ');
console.log(fruits);  // Output: ['apple', 'banana', 'cherry']
```

### Trimming
`trim()`

### Padding
`padStart()`, `padEnd()`


#### Example: Reversing a String

You can reverse a string by splitting it into an array of characters, reversing the array, and then joining it back into a string.

```js
const text = 'Hello';
const reversed = text.split('').reverse().join('');
console.log(reversed);  // Output: olleH

```