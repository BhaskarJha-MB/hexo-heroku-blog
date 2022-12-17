---
title: Path Module in Node.js
---

# Path Module in Node.js

## Introduction:

The path module is an inbuilt module in Node.js which provides the means to deal with the path of files and folders. The utilities provides by it are very useful in Node.js projects as the users who download a project and use may not have the exact same directory structure and thus the absolute path of the resources the project uses may be different. Also, different operating systems deal with paths differently. Hence to remove any inconsistencies in the path for different users or different operating systems, the path module is used.

The path module can be used by requiring the path in the file as follows:
```
const path = require('path');
```

The path module has different methods to deal with different use cases in a project. Two of the important methods of the path module are **path.join()** and **path.resolve()**.

## The path.join() method:

This method is responsible for joining the path segments provided in its argument. The arguments passed in this method are joined together using the platform-specific delimiter and the result is normalised. For example:
```
console.log(path.join("src","server","index.js"));
//Output: src/server/index.js
```
In the above example, even if the arguments did not have the platform-specific separator, the join method automatically appended the separator and created the path.

Following is the example of normalisation done by the join method when an argument has an incorrect separator.
```
console.log(path.join("src","server","\/index.js"));
//Output: src/server/index.js
```
The path module only accepts string arguments and if an argument is not a string, it throws a TypeError as follows:
```
console.log(path.join("src","server",1,"index.js"));
//TypeError [ERR_INVALID_ARG_TYPE]: The "path" argument must be of type string. Received type number (1)
```
In projects, to remove inconsistencies in the absolute path of files, the join method can be used as follows :
```
console.log(path.join(__dirname,"src/index.js"));
//Output: /home/xyz/project/src/index.js
```
This makes sure that a project works the same in different systems regardless of the root directory structure.

## The path.resolve() method:

This method takes a path or multiple paths as arguments and joins them into an absolute path. It works as follows:
```
console.log(path.resolve("src/index.js"));
//Output: /home/xyz/project/src/index.js
```
Similarly, joining multiple paths works as follows:
```
console.log(path.resolve("src","index.js"));
//Output: /home/xyz/project/src/index.js
```
Suppose a project uses an absolute file path. When downloaded or cloned in another system, the absolute file path will be different that the original project which may result in the project not working properly. Hence, path.resolve() can be used in the project to get the absolute file path without hardcoding it so that the project becomes compatible with other systems. For example:
```
console.log(path.resolve(__filename));
//Output: /home/xyz/project/index.js
```
Similarly, to get the absolute path of a file independent of the system directory structure: 
```
console.log(path.resolve(__dirname,"src/index.js"));
//Output: /home/xyz/project/src/index.js
```
## Difference between path.join() and path.resolve() methods:

Although both path.join() and path.resolve() are used to work with the file paths, they differ in some aspects:
- path.resolve() returns the absolute path of the file or directory whereas path.join() just joins the path segments together and returns it.
- path.resolve() resolves each path segment with the root whereas path.join()  just joins the path segments with the OS-specific separator.
- path.resolve() returns the absolute path of the directory if no argument is provided whereas path.join() returns '.' representing the current working directory in case of no arguments.