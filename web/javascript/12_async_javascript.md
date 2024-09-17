## 1. Async JS
---
Asynchronous: Code run at different times in a non-blocking way

Simple using callback functions does not mean that code is asynchronous.
For example, Array.map takes a callback argument, however, it does not execute asynchronously.


img.src is async (images can be large).
That is why, we use an addEventListener for the load event for the image.

## 2. AJAX
---
AJAX calls are the most important of part of Async JS.

AJAX = Asynchronous Javascript And XML

With AJAX calls, we can request data from web servers dynamically.


## 3. APIs
---
Application Programming Interface.
Allows applications to talk to each other.
For example, DOM API and Geolocation API.


## 4. XMLHttpRequest
---
```js
const request = new XMLHttpRequest();
```
## 5. Requests and Responses
---
```js
//Create Request

const request = new XMLHttpRequest();

//Open Request

request.open('GET', `https://restcountries.com/v3.1/name/${country}`);

//Send Request

request.send();


//Deserialize Response Json
const [data] = JSON.parse(this.responseText);

console.log(data);
```

## 6. Callback Hell
---



## 7. Promises and Fetch API
---
Placeholder for future value.

### Promise Lifecycle


**Pending** ------- async task ------->  **Settled**   


From **Settled**, it can either ---------> **Rejected**
or, --------------> **Fulfilled**

### fetch api 
fetch API builds the promise and returns the value when available.

**Build Promise**       ->      **Consume Promise** 

## 8. Consuming Promises
---
```js

 const getCountryData = function (country) {
   fetch(`https://restcountries.com/v3.1/name/${country}`)
     .then(function (response) {
       console.log(response);
       return response.json(); //response.json() will also return a Promise
     })
     .then(function (data) {
       console.log(data);
       renderCountry(data[0]);
     });
 };

```

## 9. Chaining Promises
---


## 10. Handling Rejected Promises
---



## 11. Throwing Errors Manually
---


## 12. Event Loop (async behind the scenes)
---
Callbacks and Promises have separate queues.
Event loop checks both queues for any updates.
Promises have priority. Callbacks get executed after promises.
```javascript
console.log('Test start');

setTimeout(() => console.log('0 sec timer'), 0);

Promise.resolve('Resolved promise 1').then(res => console.log(res));

  

Promise.resolve('Resolved promise 2').then(res => {

for (let i = 0; i < 1000000000; i++) {}

console.log(res);

});

  

console.log('Test end');


//Both promises are executed before callbacks.
```


## 13. Building a simple Promise
---

### Building a basic promise
```javascript
const lotterypromise = new Promise(function(resolve, reject) {

	console.log("Lottery draw is happening");
	
	setTimeout(function (){
		if(Math.random() >= 0.05){
			resolve("You win");
			}
		else {
			reject( new Error("You lost your money"));
		}, 2000);
	

	
	}


	lotterypromise.then(res=>console.log(res)).catch(err => console.error(err));
});

```

### Promisifying a setTimeout function:

```javascript
const wait = function(seconds) {
	const returnPromise = new Promise(function(resolve){
		setTimeout(resolve, seconds * 1000 )
	});
}



wait(2).then(() => {
	console.log("I waited by 2 seconds.");
	return wait(1);
}).then(() => console.log("I waited for 1 second"));

```
## 14. Consuming Promises
---


## 15. try, catch
---


## 16. Returning values from Async Functions
---


## 17. Running Promises in Parallel
---


## 18. Other Promise Combinators:
---
### allSettled

### any

