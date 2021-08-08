## Using Async.Js autoInject method to create complex asynchronous calls

Async.JS is a useful library to manage asynchronous JavaScript calls. It can be used in wide variety of ways to solve many common problems.

The following example shows you on how to use **async.autoInject** to combine many inter dependent asynchronous functions seamlessly

```
const async = require('async');

/**
 * adds 3 to a number after 2 seconds
 * @param text
 * @returns {Promise<unknown>}
 */
const add3 = (number = 1) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(number + 3)
        }, 2000)
    })
}

/**
 * Multiplies after 2 seconds
 * @param a
 * @param b
 * @returns {Promise<unknown>}
 */
const mulitply = (a,b) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(a*b)
        }, 2000)
    })
}

/**
 * A control flow using async.autoInject
 * @type {string}
 */
const timerId = 'asyncAuto'
console.time(timerId);
async.autoInject({
    fivePlus3: async () => add3(5),
    onePlus3: async () => add3(1),
    multiplyTheAboveTwoProps: async (fivePlus3,onePlus3) => mulitply(fivePlus3,onePlus3), // takes the resolved values of the fivePlus3,onePlus3
    add3ToFinal: async (multiplyTheAboveTwoProps) => add3(multiplyTheAboveTwoProps), // takes the resolved value of  multiplyTheAboveTwoProps
    add3TofivePlus3: async (fivePlus3) => add3(fivePlus3) // takes the resolved value of fivePlus3
}).then(r => {
    console.timeEnd(timerId);
    console.log('finished controlled flow', r)
})
```

 Find a working example => [RunKit](https://runkit.com/sebinpj/async-js-examples-for-es2017---async-auto) 


> PS: This is the first time i ever wrote anything
