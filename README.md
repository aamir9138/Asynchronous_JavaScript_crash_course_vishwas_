# Asynchronous*JavaScript_crash_course_vishwas*

## Topics to cover

- Timeouts and Intervals
- Callbacks
- Promises
- async await
- Event loop

## Async JavaScript - What and Why?

In its most basic form, Javascript is a `synchronous, blocking, single-threaded` language

### Synchronous

- If we have two functions which log messages to the console, code executes top down, with only one line executing at any given time

```
function A() {
  console.log('A')
}

function B() {
  console.log('B')
}

A()
B()

// Logs A and then B
```

### Blocking

- No matter how long a previous process takes, the subsequent processes won't kick off until the former is compoleted
- if function A had to execute an intensive chunk of code, javascript has to fimish tat wihout moving on to function B. Even if that code takes 10 seconds or 1 minute
- web app runs in a browser and it executes an intensive chunk of code without returning control to the browser, the browser cas appear to be frozen

### Single-threaded

- A thread is simply a process that your javascript program can use to run a task
- Each thread can only do one task at a time
- javascript has only one thread called the `main thread` for executing the code

### problem with synchronous, blocking, single-threaded model of javascript

- `fetchDataFromDB('endpoint')` could take 1 second or even more
- During that time, we can't run any further code
- JavaScript, if it simply proceeds to the next line without waiting, we have an error because data is not what we expect it to be
- we need a way to have asynchronous behaviour with javascript

### Async Javascript - How?

- Just javascript is not enough
- we need new pieces which are outside of Javascript to help us write asynchronous code which is where web browsers come into play
- Web browsers define functions and APIs that allow us to register functions that should not be executed synchronously, and should instead be invoked asynchronously when some kind of event occurs
- For example, that could be the passage of time(setTimeout or setInterval), the user's interaction with the mouse (addEventListener), or the arrival of data over the network (callbacks, promises, async-await)
- you can let your code do several things at the same time without stopping or blocking your main thread

### Traditional methods javascript has available for running code asynchronously

- after a set time period elapsed or in other words (`setTimeout()`)
- at regular intervals of time or in other words (`setInterval()`)

### setTimeout()

```
setTimeout(function, duration, param1, param2, ...)

function greet(){
  console.log('Hello')
}

setTimeout(greet, 2000)
// Logs 'Hello' to the console after 2 seconds
```

we can also pass a parameter

```
function greet(name){
  console.log('Hello ${name}')
}

setTimeout(greet, 2000, 'Vishwas')
// Logs 'Hello Vishwas' to the console after 2 seconds
```

### clearing a settimeout

- to clear a timeout, you can use the clearTimout() method passing in the identifier returned by setTimeout as a parameter

```
function greet(){
  console.log('Hello')
}

const timeoutId = setTimeout(greet, 2000)
clearTimeout(timeoutId)
//nothing is logged to the console.
```

- A more practical scenario is clearing timeouts when the component is unmounting to free up the resources and also prevent code from incorrectly executing on an unmounted component.

## setInterva()

- it is same as setTimeout() but it will repeat a task forever every set interval of time.

```
setInterval(function, duration, param1, param2, ...)

function greet(){
  console.log('Hello')
}

setInterval(greet, 2000)
// Logs 'Hello' to the console after every 2 seconds
```

### clearing a setInterval

- clearInterval() method is used for this

```
function greet(){
  console.log('Hello')
}

const intervalId = setInterval(greet, 2000)
clearInterval(intervalId)
```

### Noteworthy points

1. Timers and intervals are not part of javascript itself. They are implemented by the browser.
2. setTimeout and setInterval are basically names given to that functionality in javascript
3. duration parameter is the minimum delay, not guaranteed delay.
4. mentioning `2000` for time can take more than 2 sec depending if it finishes the job late
5. it is possible to achieve the same effect as `setInterval` with a recursive setTimout

```
setTimeout(function run(){
  console.log('Hello')
  setTimeout(run, 100)
}, 100)
```

Duration is guaranteed between executaions. Irrespective of how long the code takes to run, the interval will remain the same

6. on the other hand for the setInterval

```
setInterval(function run(){
  console.log('Hello')
}, 100)
```

The duration interval includes the time taken to execute the code you want to run. The code takes 40ms to run, the interval in 50ms. the code takes 50ms to run the interval is 50ms 7. if code can take longer to run the time interval itselt? Recursive setTimeout is the best than the setInterval() 8. with recursive timeout we can calculate a different delay before running each iteration 9. setInterval is always a fixed interval duration

## Callbacks

- In javascript, function are first class objects
- A function can also be returned as values from other functions

```
function greet(name){
  console.log(`Hello ${name}`)
}

function higherOrderFunction(callback){
  const name = 'Vishwas'
  callback(name)
}

higherOrderFunction(greet)
```

- Any function that is passed as an argument to another function is called a `callback function` in Javascript

## Higher order function

- The function which accepts a function as an argument or returns a function is called a higher order function

## Callbacks function types

1. synchronous Callback functions
2. Asynchronous Callback functions

### Synchronous callbacks

A callback which executed imediately is called a synchronous callback

```
function greet(name){
  console.log(`Hello ${name}`)
}

function higherOrderFunction(callback){
  const name = 'Vishwas'
  callback(name)
}

higherOrderFunction(greet)
```

The below higher order function are showing the higherorderfunction with synchronous callbacks

```
let numbers = [1,2,3,4,5,6]
numbers.sort((a,b) => a -b)
numbers.map(n => n*2)
numbers.filter(n => n%2 === 0)
```
