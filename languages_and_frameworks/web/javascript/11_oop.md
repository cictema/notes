## OOP in javascript
---
No Real Classes As In Other Languages


## Constructor Functions
---
```js
	const Person = function (firstName, lastName){
		this.firstname = firstName;
		this.lastname = lastname;
	}
```
## new operator
---
```js

	const Jonas = new Person("Jonas", "Schedtmann");
```
## Prototypes
---
```js

Person.protoype.calcAge = function(birthYear){
	console.log(2024 - this.birthYear);
}
```

#### Check Prototype:
```js
console.log(jonas.__proto__ === Person.protoype); //true
console.log(Person.prototype.isPrototypeOf(Person));//false
console.log(Person.prototype.isPrototypeOf(jonas));//true
```

Prototype of the Jonas object is the prototype property of the constructor function.
That is, Person.prototype is not the prototype of person, but the prototype of every object created from it.

#### Own Property
If property is defined in the constructor function, then it is an Own property.
```js
console.log(jonas.hasOwnProperty("firstname")); //true
console.log(jonas.hasOwnProperty("calcAge")); //false
```

## Prototypal Inheritance
---
If JS cannot find a property in its own Object prototype, it will look up the prototype chain.

The Jonas object created the calcAge method from its prototype. (delegated)


## Prototype Chain
---
1. jonas -> Person.prototype -> Object.prototype

2. Object.prototype = null (top of prototype chain)


## Prototypal Inheritance on Built-in Objects
---
```js
const arr = [1,2,3,4]
console.log(arr___proto);
//will print map, join, push, etc
```
## ES6 Classes
---
### Class Expression 
```js
const PersonClass = class {}

```
### Class Declaration
```js
class PersonClass{
	constructor(firstName, lastName, age){
		this.firstname = firstName;
		this.lastname = lastName;
		this.age = age;
	}
	//Methods
	calcTimeOfDeath(){
		console.log("You will die in " + (2096 - age) + years);
	}
	greet(){
		console.log("Hi, " + this.firstname);
		}
}

//Declare new Object
const Jonas = new PersonClass("Jonas", "Schedtmann", 34);
Jonas.calcTimeOfDeath()


//Check Prototype
console.log(Jonas.__proto__ === PersonClass.prototype); //true

```

1. Classes are not hoisted.
2. Classes are first-class members
3. Classes are executed in strict mode.

### Getter
```js
class PersonClass{
	constructor(firstName, lastName, age){
		this.firstname = firstName;
		this.lastname = lastName;
		this.age = age;
	}
	//Getter
	get name(){
		return this.firstname+ " " +this.lastname;
	}
} 

const jonas = new Person("Jonas", "Schedtmann", 34);

//Use Getter
console.log(jonas.name);
```
### Setter
```js
class PersonClass{
	constructor(firstName, lastName, age){
		this.firstname = firstName;
		this.lastname = lastName;
		this.age = age;
	}
	//Setter
	set fullname(){
		this._fullname =  this.firstname+ " " +this.lastname;
	}
	//Getter
	get fullname(){
		return this._fullname;
	}
} 

const jonas = new Person("Jonas", "Schedtmann", 34);

//Use Setter
console.log(jonas.name);
```
## Static Methods
---
```js
Array.from(document.querySelector('h1')); // can be used
[1,2,3].from() //cannot be used

```
1. from is attached to the Array constructor.
2. from is said to be in the Array namespace.
3. used as helper and util methods.

```js
class PersonClass{
	constructor(firstName, lastName, age){
		this.firstname = firstName;
		this.lastname = lastName;
		this.age = age;
	}
	//Setter
	set fullname(){
		this._fullname =  this.firstname+ " " +this.lastname;
	}
	//Getter
	get fullname(){
		return this._fullname;
	}
} 

//Static method
PersonClass.hey = function(){
	console.log('Hey there');
}

Person.hey(); // can be called

const jonas = new Person("Jonas", "Schedtmann", 34);
jonas.hey() // won't work

```

### Defining Static Method Inside a Class
```js
class PersonClass{
	constructor(firstName, lastName, age){
		this.firstname = firstName;
		this.lastname = lastName;
		this.age = age;
	}
	// Instance Methods - will be added to .protoype property
	calcTimeOfDeath(){
		console.log("You will die in " + (2096 - age) + years);
	}
	greet(){
		console.log("Hi, " + this.firstname);
		}
	//Setter
	set fullname(){
		this._fullname =  this.firstname+ " " +this.lastname;
	}
	//Getter
	get fullname(){
		return this._fullname;
	}
	//Static Method Definitoin - will not be added to .prototype property
	static hey(){
		console.log('Hey there');
	}
} 
```
## Object.create
---
```js
class PersonProto{
	constructor(firstName, lastName, birthyear){
		this.firstname = firstName;
		this.lastname = lastName;
		this.birthyear = 1993
	}
	// Instance Methods - will be added to .protoype property
	calcAge(){
		console.log(2037 - this.birthyear)
	}
	greet(){
		console.log("Hi, " + this.firstname);
		}
	//Setter
	set fullname(){
		this._fullname =  this.firstname+ " " +this.lastname;
	}
	//Getter
	get fullname(){
		return this._fullname;
	}
	//Static Method Definitoin - will not be added to .prototype property
	static hey(){
		console.log('Hey there');
	}
} 


// Basic Object Declaration
const jonas = new Person("Jonas", "Schedtmann", 1992);


//Using Object.create
const steven = Object.create(PersonProto);
steven.firstname = "Steven";
steven.lastname = "Mark";
steven.birthyear = 1992;

steven.calcAge(); // will work


```

When we use new() to create a new object, it automatically initiates the __proto__ property to the constuctor's prototype.

In Object.create, we manually assign that.

### Init to initiatlize values when using Object.create

```js
class PersonProto{
//Init
	init(firstName, lastName, birthyear){
		this.firstname = firstName;
		this.lastname = lastName;
		this.birthyear = 1993
	}
	// Instance Methods - will be added to .protoype property
	calcAge(){
		console.log(2037 - this.birthyear)
	}
	greet(){
		console.log("Hi, " + this.firstname);
		}
	//Setter
	set fullname(){
		this._fullname =  this.firstname+ " " +this.lastname;
	}
	//Getter
	get fullname(){
		return this._fullname;
	}
	//Static Method Definitoin - will not be added to .prototype property
	static hey(){
		console.log('Hey there');
	}
} 


// Basic Object Declaration
const jonas = new Person("Jonas", "Schedtmann", 1992);


//Using Object.create
const steven = Object.create(PersonProto);
steven.init("Steven", "Mark", 1992);

steven.calcAge(); // will work
```

## Inheritance between Classes:
---
### Constructor Functions
### ES6 Classes
### Object.create




## Encapsulation
---
### Protected Properties and Methods

### Private Class Fields and Methods



## Chaining Methods
---


