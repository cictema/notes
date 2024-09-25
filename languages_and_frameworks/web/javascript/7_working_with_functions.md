## Default Parameters
---


## Passing by Value vs Reference
---


## First Class vs Higher Order Functions
---

### First Class
```js
function newFunc(var1){
	console.log(var1);
}
```
### Higher Order
```js
function higherOrderFunc(var1){
	const returnFunc = function () {
		result = var1+2;
		return result;
	}
	return returnFunc;
}

```

Higher Order Functions are functions that use other functions, either to return, call, or callback.

## Functions accepting Callback Functions
---


## call
---


## apply
---


## bind
---


## Immediately Invoked Function Expressions (IIFE)
```js
(function(){})();

```


---


## Closures



---

