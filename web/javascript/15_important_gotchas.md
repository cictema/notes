
# foreach vs map
---
```javascript
const numbers = [1, 2, 3];
numbers.forEach(num => console.log(num * 2));  // Logs: 2, 4, 6
// But it does not return anything or modify the `numbers` array.

```

```javascript
const numbers = [1, 2, 3];
const doubledNumbers = numbers.map(num => num * 2);
console.log(doubledNumbers);  // Outputs: [2, 4, 6]

```

Both operations do not change the original array.

### Key Differences:
1. **Return value**:
    
    - `forEach()`: Doesn't return anything (`undefined`).
    - `map()`: Returns a new array with the transformed values.
2. **Mutation**:
    
    - `forEach()`: Can mutate the original array if necessary, though it's not always encouraged.
    - `map()`: Does not modify the original array; it returns a new array.
3. **Use case**:
    
    - `forEach()`: Best for when you need to perform side effects or actions on each element (e.g., logging or mutating).
    - `map()`: Best for when you want to transform or return a new array based on the original array.


# Scope and Hoisting for Variables (let, const, var)
---
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
// Outputs: 3, 3, 3 (due to var being function-scoped)

for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 1000);
}
// Outputs: 0, 1, 2 (due to let being block-scoped)
```
### Explanation:
- **`var` is function-scoped**, meaning that `i` is shared across all iterations of the loop. The variable `i` is declared once for the entire function or global scope.
- By the time the `setTimeout()` callbacks are executed (after 1000ms), the loop has already completed, and `i` is equal to `3`. The value of `i` doesn't "freeze" at each iteration.
- As a result, all the `setTimeout` callbacks log `3`, which is the final value of `i` after the loop finishes.
- **`let` is block-scoped**, meaning that a new instance of `j` is created for each iteration of the loop. Each iteration gets its own copy of `j`, and its value is preserved for that particular iteration.
- When the `setTimeout()` callbacks are executed (after 1000ms), each callback has access to its own `j` value from the iteration in which it was created.
- As a result, the output is `0`, `1`, `2`, as each `setTimeout` has access to the correct value of `j` for that specific iteration.


# Primitive vs Reference Types
---
```js
let obj = { a: 1 };
function modify(o) {
  o.a = 2;
  o = { a: 3 };
}
modify(obj);
console.log(obj.a); // Outputs: 2

```
**Explanation**: Inside the function, `o.a` modifies the original object because objects are passed by reference. However, reassigning `o` creates a new object, but this new object doesn’t affect the original reference outside the function.

### Common Reference Types in JavaScript:

1. **Objects** (including arrays and functions)
2. **Arrays**
3. **Functions**

#### Objects As Reference Types:
```js
let obj1 = { name: "Alice" };
let obj2 = obj1;  // obj2 now holds a reference to the same object as obj1

obj2.name = "Bob";

console.log(obj1.name); // Outputs: "Bob"
console.log(obj2.name); // Outputs: "Bob"

```
#### Arrays As Reference Types:
```js
let arr1 = [1, 2, 3];
let arr2 = arr1;  // arr2 holds a reference to the same array as arr1

arr2.push(4);

console.log(arr1); // Outputs: [1, 2, 3, 4]
console.log(arr2); // Outputs: [1, 2, 3, 4]

```
#### Functions As Reference Types
```js
function greet() {
  console.log('Hello!');
}

let sayHello = greet; // Assign the reference to `sayHello`

greet = function() {
  console.log('Hi there!');
};

sayHello();  // Outputs: 'Hello!' (Still refers to the original function)
greet();     // Outputs: 'Hi there!' (Refers to the modified function)


```

# Type Coercion
---
```js
console.log([] + []);      // Outputs: ''
console.log([] + {});      // Outputs: '[object Object]'
console.log({} + []);      // Outputs: 0 (in non-console environments)

```

**Explanation**: JavaScript performs implicit type conversions, and adding arrays or objects can result in unexpected string concatenation or type coercion to primitives.

Refer: [type_coercion](type_coercion)



# Equality (`==` vs `===`)
```js
console.log(0 == false);   // true
console.log(0 === false);  // false
console.log(null == undefined);  // true
console.log(null === undefined); // false

```

**Explanation**: The `==` operator performs type coercion before comparison, while `===` requires both type and value to be identical.

# Floating-Point Precision
---
```js
console.log(0.1 + 0.2); // Outputs: 0.30000000000000004

```
- **Explanation**: JavaScript uses IEEE 754 floating-point arithmetic, which can lead to rounding errors in certain calculations.

# Object Property Access
---
```js
const obj = { a: 1 };
console.log(obj.b);       // undefined (doesn't throw an error)
console.log(obj.b?.c);    // undefined (safe access with optional chaining)

```

**Explanation**: Accessing non-existent properties returns `undefined` rather than throwing an error. Optional chaining (`?.`) is used to avoid errors in deeply nested objects.

### Optional Chaining (`?.`):

- **Purpose**: Optional chaining allows you to safely access deeply nested properties without worrying about whether intermediate properties exist or are `undefined`/`null`.
    
- Without optional chaining, attempting to access `obj.b.c` when `b` is `undefined` would throw a `TypeError` ("Cannot read property 'c' of undefined").

# `this` Keyword
---
```js
const person = {
  name: 'Alice',
  greet: function() {
    console.log(this.name);
  },
};
const greet = person.greet;
greet(); // undefined (because 'this' refers to global object)

```

**Explanation**: When a method is detached from its object and called as a standalone function, `this` refers to the global object (or `undefined` in strict mode), not the original object.

Refer: [this_keyword](this_keyword)

# Promise and Async/Await
---
```js
async function test() {
  return 1;
}

console.log(test()); // Outputs: Promise {<fulfilled>: 1}
```

**Explanation**: An `async` function always returns a promise, even if it seems to return a value directly. You need to `await` or `.then()` the promise to access its value.


# Array Mutations
---
```js
let arr = [1, 2, 3];
let newArr = arr.map(x => x * 2);
console.log(arr);    // [1, 2, 3] (not mutated)
console.log(newArr); // [2, 4, 6] (new array)

arr.push(4);
console.log(arr);    // [1, 2, 3, 4] (mutated)

```
**Explanation**: Methods like `map`, `filter`, and `slice` return a new array, while methods like `push`, `pop`, and `splice` mutate the original array.

# `delete` Operator and Arrays
---
```js
let arr = [1, 2, 3];
delete arr[1];
console.log(arr);    // [1, empty, 3]


```
**Explanation**: The `delete` operator removes the property from an object, but for arrays, it leaves a hole (`empty`) rather than shifting the elements. Use `splice` for proper removal.

# NaN
---
```js
console.log(NaN === NaN);   // false
console.log(isNaN(NaN));    // true

```

# Prototype Chain
---
```js
const obj = { a: 1 };
Object.prototype.b = 2;
console.log(obj.b); // Outputs: 2
```

**Explanation**: If a property or method is not found directly on an object, JavaScript looks up the prototype chain to find it.


# Array `sort()` on Numbers
---
```js
let numbers = [10, 5, 20, 15];
numbers.sort();
console.log(numbers);  // [10, 15, 20, 5]

```

**Explanation**: The `sort()` method converts numbers to strings and sorts them lexicographically (by their Unicode value). To correctly sort numbers, you need to pass a comparison function:

```js
numbers.sort((a, b) => a - b);
console.log(numbers);  // [5, 10, 15, 20]

```
