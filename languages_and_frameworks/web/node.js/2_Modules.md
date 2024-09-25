# Importing
---
```node
const fs = require(`fs`);

fs.writeFileSync("notes.txt", "This is a new file.");
fs.writeAppendSync("notes.txt", "This is an appended string.")
```

# Exporting
```node
module.exports = <name>

```

# npm
```node
npm init //initialize local npm files
npm install <name>@version //install local npm packages
npm install -g <name> //globally
```