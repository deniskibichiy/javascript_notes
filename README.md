# javascript_notes
A tactical reference of JavaScript build for my future self.
# Asynchronous functions
* Functions that can happen in the background while the rest of the code executes
# Callback
* A function that is passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action
## Synchronous callbacks:
* They are called immediately after the invocation of the outer function, with no intervening asynchronous tasks. Examples are functions that are passed to `array.prototype.map()` and `array.prototype.forEach()`
## Asynchronous callbacks:
* They are called at some point later after an asynchronous tasks operation has completed. 
# Promise
* An object that might produce some value at some point in the the future. 
* Constructor syntax for a promise object:
```js
let promise = new Promise (function (resolve, reject) {
    //executor (the producing code)
});
```
## Executor
* A function passed into `new Promise` is called the executor and it runs automatically and contains the producing code which eventually produce the result.
* When the executor obtains the result, it calls one of the callbacks, which can either be the `resolve` or `reject`
* In practice, the executor usually does something asynchronously and calls resolve/reject after some time, but doesn't have to. Resolve/reject can also be called immediately. 
## `resolve`
* Called if the job is finished successfully
## `reject`
* Throws an error object if an error has occurred.
* The executor should call `reject` with any type of argument just like `resolve` but it is recommended to use `Error` objects or objects that inherit from `Error`.
# `settled`
* This refers to a promise that has either been resolved or rejected. 
* The executor can only call only call one reject or one resolve. Any state change is final. 
* `resolve` and `reject` also expect only one argument and will ignore additional arguments. 
# Properties of the promise object
## `state` and `result`
* The properties `state` and `result` of the Promise object are only internal and we can't directly access them. 
## Consumers `.then` and `catch`
* A promise object serves as a link between the executor and the consuming functions, which will receive the result or error. 
* The methods `.then` and `.catch` are consuming functions that can be registered (subscribed) using the methods. 
## `then`
* Tells a function to wait until a promise is resolved. 
* Syntax:
```js
    promise.then (
        function (result) { /* handle a successful result */},
        function (error) {/* handle an error */}
    );
```
* `.then` can be supplied with only one function argument if we are only interested with successful completions. 

## `catch`
* If we are interested only in errors, then we can use `null` as the first argument
`.then(null, errorHandlingFunction)` or we can use `.catch(errorHandlingFunction)`.
* `.catch(f)` is similar to `.then(null, f)`, it is a complete analog, only that it is a shorthand. 

## Finally
* `.finally(f)` is similar to `.then(f,f)` in the sense that `f` runs always, when the promise is settled, be it rejected or resolved. 
* `finally` is used to set up a handler for performing cleanup/finalizing after the previous operations are complete, to stop loading an indicator or close no longer needed connections. 
```js
    new Promise ((resolve, reject) => {
        //do something that takes time, and then call resolve or maybe reject
    })

    .finally(() => stop loading indicator)
    .then (result => show result, err => show error)

```

## `finally(f)` vs `then(f,f)`
* A `finally` handler has no arguments. In `finally` we don't know whether the promise is successful or not. The only concern here is to perform general finalizing procedures. 
* A `finally` handler passes through the result or error to the next suitable handler. 
* A `finally` hander shouldn't return anything. If it does, the returned value is silently ignored. The only exception is when a `finally` handler throws an error which will go to the next handler, instead of any previous outcome. 



