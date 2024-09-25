
## String Concatenation
```js
console.log(1 + '2');  // '12'
console.log('3' + 4 + 5);  // '345'
console.log(4 + 5 + '6');  // '96'

```

## Booleans in Arithmetic Operations
```js
console.log(true + true);  // 2
console.log(true - false);  // 1
console.log(true * 2);  // 2
console.log(false + 1);  // 1

```
**Explanation**: When used in arithmetic operations, `true` is coerced to `1` and `false` to `0`. So:

- `true + true` is `1 + 1` (which equals `2`),
- `true - false` is `1 - 0` (which equals `1`),
- and `false + 1` is `0 + 1` (which equals `1`).


## Falsy Values in Comparisons
```js
console.log(null == undefined);  // true
console.log(null === undefined); // false
console.log(false == 0);  // true
console.log('' == 0);  // true
console.log([] == false);  // true

```
**Explanation**:

- `null == undefined` is `true` because JavaScript considers them loosely equal (they represent an absence of value).
- `false == 0`, `'' == 0`, and `[] == false` are all `true` because JavaScript converts both sides to a number (`false`, `''`, and `[]` all coerce to `0`).
- However, `null === undefined` is `false` because `===` checks both value and type, and they are of different types.

## Objects and Type Coercion
```js
console.log({} + []);  // 0
console.log([] + {});  // '[object Object]'
console.log({} + {});  // '[object Object][object Object]'

```

## Adding Arrays and Objects
```js
console.log([1, 2] + [3, 4]);  // '1,23,4'
console.log({} + 1);  // 1
```

**Explanation**:

- When adding two arrays, JavaScript first coerces them into strings, so `[1, 2] + [3, 4]` results in `'1,23,4'` (as both arrays are converted to their string representations: `'1,2'` and `'3,4'`).
- `{}` + `1` results in `1` because of how JavaScript interprets an empty block `{}` and then the number `1` after it.

## Type Coercion with `==`
```js
console.log([] == '');  // true
console.log([0] == 0);  // true
console.log([null] == 0);  // true
console.log([undefined] == 0);  // true
console.log('0' == false);  // true

```
**Explanation**:

- `[] == ''` is `true` because both `[]` and `''` are coerced to empty strings.
- `[0] == 0`, `[null] == 0`, and `[undefined] == 0` are `true` because the array is coerced to a primitive (string or number), and JavaScript converts the single elements to `0`.
- `'0' == false` is `true` because `'0'` is coerced to `0`, and `false` is also coerced to `0`.

## Comparing Objects and Arrays
```js
let a = {};
let b = {};
console.log(a == b);  // false
console.log(a === b);  // false

let c = [];
let d = [];
console.log(c == d);  // false
console.log(c === d);  // false

```

## `null` and `undefined` in Arithmetic Operations
```js
console.log(null + 1);  // 1
console.log(undefined + 1);  // NaN
```

**Explanation**:

- `null` is coerced to `0`, so `null + 1` results in `1`.
- `undefined` is coerced to `NaN` (Not-a-Number) when involved in arithmetic, so `undefined + 1` results in `NaN`.

## Subtraction and Division Type Coercion
```js
console.log('10' - 5);  // 5
console.log('10' / 2);  // 5
console.log('10' * 2);  // 20
console.log('10' + 5);  // '105'
```

**Explanation**:

- The `-`, `/`, and `*` operators trigger **numeric coercion**, so strings like `'10'` are converted to numbers before the arithmetic operation is performed.
- However, the `+` operator does **string concatenation** when one operand is a string, so `'10' + 5` results in `'105'` (not arithmetic addition).

## Boolean Coercion in Logical Operators
```js
console.log('' || 'default');  // 'default'
console.log('hello' && 'world');  // 'world'
console.log(0 || 42);  // 42
console.log(1 && 2);  // 2
```
**Explanation**:

- The `||` (logical OR) operator returns the first **truthy** value. If `''` (an empty string, which is falsy) is encountered, the second operand (`'default'`) is returned.
- The `&&` (logical AND) operator returns the last **truthy** value, so `'hello' && 'world'` returns `'world'`.
