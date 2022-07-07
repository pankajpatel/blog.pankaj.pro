## Today's npm package: child-process-ext

Today's npm package is [child-process-ext](https://www.npmjs.com/package/child-process-ext) 

When creating CLI tools, you might wanna use other CLI tools and not have the luxury of npm package around those tools.

In such cases, you would need to create a child process with specific CLI command and get the result from that process.

And to create child process, we have node.js's `spawn` function

Currently spawn functions are not promisified and today's package [child-process-ext](https://www.npmjs.com/package/child-process-ext) will let you do the promisified work on spawned processes.

Here's how to use it:

```js
const spawn = require('child-process-ext/spawn');

const proc = spawn('ls');

proc.then(op => console.log(op.stdoutBuffer.toString()))
```

and generates following output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1623431006113/qZHZvbPkcA.png)

Stay tuned for tomorrow's package of the Day.