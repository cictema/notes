
# classes, attributes, and id
---
## class = "modal"
```js
.modal
```

## id = "btnPress"
```js
#id
```

## attributes
```js
document.querySelector(".modal").style.backgroundColor = "red"
```


# querySelector
---

```js
document.querySelector(".modal").style.backgroundColor = "red"
```


# querySelectorAll
---
```js
const modals = document.querySelectorAll(".modal").style.backgroundColor = "red"

for (let i =0; i< modals.length < i++){
	modals[i].classList.remove("hidden");
}
```

## classList
---
```js
const modals = document.querySelectorAll(".modal").style.backgroundColor = "red"

for (let i =0; i< modals.length < i++){
	modals[i].classList.remove("hidden");
}
```

## addEventListener
---
```js
const modals = document.querySelectorAll(".modal").style.backgroundColor = "red"

for (let i =0; i< modals.length < i++){
	modals[i].addEventListener('click', function (e) => {
		modals[i].classList.remove("hidden");
		modals[i].classList.remove("hidden");
	
	});
}

```


## Keypress Events
---
```js
keyup // When a key is released.
keypress // When a key that produces a character value is pressed down.(e.g., letters, numbers).
keydown // When a key is pressed down.

```

