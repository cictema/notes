## strict mode
---
```js
use 'strict'; //at the top 
```

## Function Declaration Vs Expression
---
```js
//Declaration
function (var1, var2){
	//do something
}

//Expression
const newFunc = function (var1, var2){
	//do something
}
```

## Arrow Function
---
```js

// inline
const newFunc = (var1,var2) => var1+var2; 

//multiline
const newFunc = (var1, var2) => { 
	//do something
	return;
}
```


## Functions Calling Other Functions
---





## Arrays
---

```js
const arr = [1,2,4, "adf", [223,4, "adf"]]
```


## Basic Array Operations
---
### Push
```js
const arr = [1,2,3,4,5]
arr.push(6);

```
### Pop
```js
arr.pop();
```

## Objects
---
```js
//like a hashmap
const Person = {
	name: "Jonas",
	age: "32"
}
```



## Dot Vs Bracket notation
---


## Loops
---
### for
```js
for (let i= 0; i< 10; i++){
	//do something
}
```
### while
```js
let i = 0;
while(i <= 10 ){
	// do something
	i++;
}
```
### break/continue
```js 
for (let i= 0; i< 10; i++){
	if(i === 5){
	continue;
	}
	if (i === 9){
	break;}
}


let i = 0;
while(i <= 10 ){
	if(i === 5){
	continue;
	}
	if (i === 9){
	break;}
	i++;
}
```


