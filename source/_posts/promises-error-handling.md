---
title: Error handling in promises
author: Bhaskar
---

## Importance of error handling in promises

Promises are objects which hold the future value of an asynchronous operation. Promises provide an efficient way of handling errors, unlike callbacks where asynchronous error handling needs to be done at every step of the operation. With promises, a single error handler is enough for a series of chained promises. Once a promise gets rejected, the control of the code gets shifted to the nearest error handler and the next promise chains is not executed.

In the below example, the findEven(21) function returns a promise in rejected state and it gets immediately handled by the catch() handler and the next set of promises are not executed.

```
findEven(21)
.then(function(number) {
    console.log(number);
    return number;
})
.catch(function(err){
    console.log(err.message);
});


function findEven(number) {
    const promise = new Promise((resolve,reject) => {
        if(number % 2 !== 0){
            const err = new Error("The number is not even");
            reject(err);
        } else {
            setTimeout(() => {
                resolve(number);
            },3000);
        }
    });
    return promise;
}

//Output: The number is not even
```

If the error is not handled, the JavaScript engine issues a runtime error and stops the program.

```
findEven(1)
.then(function(number) {
    console.log(number);
    return number;
});


function findEven(number) {
    const promise = new Promise((resolve,reject) => {
        if(number){
            reject();
        } else {
            resolve();
        }
    });
    return promise;
}

//Output: ERR_UNHANDLED_REJECTION 
```
If there are no rejection handlers to call when a promise gets rejected, it results in an 'unhandledrejection' event and thus the error is not handled gracefully. Hence, handling errors is important in promises.

## How to handle errors in promises

Errors in promises are handled with the .catch handler. The .catch handler handles the rejected promises above it and not below it. Hence, even a single .catch handler at the end of a promise chain is enough to handle the rejection of any promise. 

Error handling in promises is done as follows:

```
promise1
.then(promise2)
.then(promise3)
.catch((err) => {
    console.error(err);
});
```

Here, the catch() method is will handle the rejection of any of promise1, promise2 or promise 3. 

The catch() method can even be present in the middle of the promise chain as follows:

```
promise1
.then(promise2)
.catch((err) => {
    console.error(err);
})
.then(promise3);
```

However, in this case, the catch() method will only handle the rejection of promise1 or promise2 and not promise3. Hence, the catch() method can be chained in the same way as then() as it also returns a promise. 

Suppose there is an addFive function which returns a promise. The promise is rejected if the number passed in the function argument is negative. If the number is not negative, the promise is resolved. Since, in the below example, the number is negative, the promise gets rejected and thus, gets handled by the catch() method as follows:

```
addFive(-2)
.then((number) => {
    console.log(number);
})
.catch((err) => {
    console.error(err.message);
})

function addFive(number){
    return new Promise((resolve,reject) => {
        if(number<0){
            const err = new Error("Number is negative");
            reject(err);
        } else {
            number += 5;
            resolve(number);
        }
    });
}

//Output: Number is negative
```




