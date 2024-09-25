# Hoisting
```js
var temp= 'hi';
function display(){
    console.log(temp);
    var temp = 'bye';
};
display();
```

### Step-by-step explanation:
1. **Global Scope:**
    - The variable `temp` is declared and initialized globally with the value `'hi'`.
2. **Inside the `display` function:**
    - When the `console.log(temp)` is executed, JavaScript is supposed to print the value of `temp`. However, due to **hoisting**, the local `var temp` declaration is hoisted to the top of the function scope, but without an assignment.
    - This results in `temp` being `undefined` at the point of `console.log(temp)` because JavaScript sees the local declaration before reaching the assignment.
3. **The `var` declaration hoists only the declaration, not the initialization.** Hence, the code behaves as if it's written like this:

```js
text = "123"
console.log(text)
var text
```
Why did we not run into an error and get `123` as an output? It’s because the variable declaration on **line 3** gets hoisted to the top.


## Arrow Function
```js
const button = document.querySelector('#pushy');

button.addEventListener('click', () => {

    console.log(this); //`this` refers to window

    this.classList.toggle('on');

});
```

In JavaScript, arrow functions do not bind their own `this`, meaning they inherit the one from the parent scope; this is also known as **lexical scoping**.

Hence, in the code above, if you display the value of `this` you’ll see that the result will be `window`.