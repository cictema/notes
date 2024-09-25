
# Getting Input
Args stored in `process.argv`

```bash
node app.js andrew

//argv[1] = node
//argv[2] = app.js
//argv[3] = andrew


//Accessed by
const name = process.argv[2]
```

# Argument Management With Yargs For Parsing
---

```node
npm install yargs
```

```bash
npm app.js add --title="My note"

```

```node
console.log(yargs.argv.title) // My note
```

### Set up Yargs command
```node
yargs.command({
	command: 'add',
	describe: 'Add a new note',
	builder: {
		title: {
			describe: 'Note title',
			demandOption: true,
			type: 'string'
		},
		body: {
			describe: 'Note body',
			demandOption: true,
			type: 'string'
		}
	},
	handler: function (argv) {
		console.log('Title: ' + argv.title)
		console.log('Body: ' + argv.body)
		}
})
```

# JSON
---
```node
JSON.stringify() //Object to JSON
JSON.parse() //JSON to Object

```


# Debug
```
node inspect
```

```node
debugger
```


Add hostname and port to chrome inspect page