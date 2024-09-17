# Linking a JS file:
---
```html
<script> </script>
```
# Values and Variables
---


# Data Types
---


# let, const, var
---


# Basic Operators
---


# Operator Precedence
---


# Strings and Template Literals
---


# if/else
---


# Type Conversion and Coercion
---


# Truthy and Falsy Values
---
### Falsy Values

There are exactly **6 falsy values** in JavaScript:

1. **`false`** – The boolean `false`.
2. **`0`** – The number zero.
3. **`-0`** – Negative zero.
4. **`""` (empty string)** – A string with no characters.
5. **`null`** – Represents the absence of any object value.
6. **`undefined`** – Indicates that a variable has not been assigned a value.
7. **`NaN` (Not-a-Number)** – Result of a failed number conversion or undefined mathematical operations.

```js
if (!false) { console.log("This is falsy"); }  // Output: This is falsy
if (!0) { console.log("This is falsy"); }      // Output: This is falsy
if (!"") { console.log("This is falsy"); }     // Output: This is falsy
if (!null) { console.log("This is falsy"); }   // Output: This is falsy
if (!undefined) { console.log("This is falsy"); } // Output: This is falsy
if (!NaN) { console.log("This is falsy"); }    // Output: This is falsy

```

### Truthy Values

All other values in JavaScript are considered truthy. This includes:

    true – The boolean true.
    Non-zero numbers (both positive and negative): 1, -1, 3.14, etc.
    Non-empty strings: "hello", "0", "false", etc.
    Objects (including empty objects): {}, [].
    Functions: function() {}.
    Symbol: Symbol().

```js
if (true) { console.log("This is truthy"); }      // Output: This is truthy
if (1) { console.log("This is truthy"); }         // Output: This is truthy
if ("hello") { console.log("This is truthy"); }   // Output: This is truthy
if ([]) { console.log("This is truthy"); }        // Output: This is truthy
if ({}) { console.log("This is truthy"); }        // Output: This is truthy
if (function() {}) { console.log("This is truthy"); } // Output: This is truthy

```
# Equality 
---
## == vs ===
 
# Boolean Logic
---


# Logical Operators
---


# switch statement
---


#  Statements and Expressions
---


# Ternary Operator
---
```js
() ? () : ()
```

# ES5, ES6, ESNext
---
Refer: [[version_differences]]

