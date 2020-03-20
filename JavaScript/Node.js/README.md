# Node.js

## Stream

### Writable stream must be closed manually if a Readable stream which is piped with the writable stream emits error during processing

**Date written: 2020-03-20**

An example code:

```javascript
const fs = require("fs");

const reader = fs.createReadStream();
const writer = fs.createWriteStream();

// piping two streams
reader.pipe(writer);

reader.on("error", error => {
  // when the readable stream emits "error" 
  // the writable stream must be closed manually like this:
  writer.close();
});
```
