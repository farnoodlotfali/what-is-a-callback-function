# What is a Callback Function

A concise guide to **callback functions in JavaScript**, covering what a callback is, how to pass one, a common mistake, synchronous and asynchronous callbacks, why we need callbacks, and how to escape callback hell.

## 📄 Contents

This guide covers:

- What a callback function is
- A simple callback example
- A common mistake: passing vs calling
- Two kinds of callbacks
- Why we need callbacks
- Why the output order changes
- What callback hell is
- Escaping callback hell
- Key takeaways

## 🔁 What is a Callback Function?

A callback function is a function passed into another function as an argument. The outer function invokes it, now or later, to complete an action.

## 💡 Example: A Simple Callback

```javascript
function callbackFunction(name) {
  console.log("Hello " + name);
}

function outerFunction(callback) {
  const name = "Sara"; // prompt() works only in browsers
  callback(name);
}

outerFunction(callbackFunction);
```

Console output:

```
Hello Sara
```

> Pass the function itself: `outerFunction(callbackFunction)`, not `callbackFunction()`.

## ⚠️ Common Mistake

Passing a function is not the same as calling it.

```javascript
// Calls callbackFunction right away
// and passes its result: undefined
outerFunction(callbackFunction());
```

Console output:

```
Hello undefined
TypeError: callback is not a function
```

```javascript
// Passes the function itself
outerFunction(callbackFunction);
```

Console output:

```
Hello Sara
```

> Parentheses call a function. Leave them off when you only want to pass it.

## 🔀 Two Kinds of Callbacks

| Kind | When it runs | Examples |
| --- | --- | --- |
| **Synchronous** | Immediately, while the outer function is still working | `map`, `filter`, `sort` |
| **Asynchronous** | Later, after an event or a delay | timers, events, requests |

```javascript
// Synchronous
const doubled = [1, 2, 3].map((n) => n * 2);
console.log(doubled); // [2, 4, 6]

// Asynchronous
setTimeout(() => {
  console.log("later");
}, 1000);
```

## ⏱️ Why Do We Need Callbacks?

JavaScript does not wait for slow work, so it needs a way to run code later. In this example, the first function starts a timer and the second one logs a message:

```javascript
function firstFunction() {
  // Simulate a code delay
  setTimeout(function () {
    console.log("First function called");
  }, 1000);
}

function secondFunction() {
  console.log("Second function called");
}

firstFunction();
secondFunction();
```

Console output:

```
Second function called
First function called
```

### Why the output order changes

JavaScript runs one thing at a time. The timer waits outside.

1. **t = 0 ms**: `firstFunction()` runs. It asks the environment to start a 1-second timer, then returns.
2. **t = 0 ms**: `secondFunction()` runs. No waiting: it logs "Second function called" right away.
3. **t = about 1000 ms**: the callback runs. The timer is due, so the callback logs "First function called".

> The delay is a minimum. The callback runs once the timer is due and JavaScript is free.

## 😵 What is Callback Hell?

Callback hell is an anti-pattern: many nested callbacks make asynchronous code hard to read and debug.

```javascript
getUser(id, (err, user) => {
  getOrders(user.id, (err, orders) => {
    getItems(orders[0].id, (err, items) => {
      getPrice(items[0].id, (err, price) => {
        console.log(price);
      });
    });
  });
});
```

- The code drifts to the right, the "pyramid of doom".
- Every level needs its own error check.
- It is hard to change, test and debug.

## 🚪 Escaping Callback Hell

| Way out | How it helps | Example |
| --- | --- | --- |
| **Name your functions** | Move each callback into its own named function to flatten the nesting | `getUser(id, onUser)` |
| **Use Promises** | Chain `.then()` calls instead of nesting callbacks inside callbacks | `getUser(id).then(...)` |
| **Use async/await** | Write asynchronous code that reads from top to bottom | `await getUser(id)` |

## ✅ Key Takeaways

1. **A function as an argument**: a callback is a function passed into another function, which invokes it to complete an action.
2. **Pass it, don't call it**: write `outerFunction(callback)`, not `outerFunction(callback())`.
3. **Sync or async**: callbacks can run right away, like `map`, or later, like timers, events and requests.
4. **Why we need them**: JavaScript keeps running instead of waiting, so slow work reports back through a callback.
5. **Callback hell**: deeply nested callbacks are hard to read, debug and maintain.
6. **Ways out**: named functions, Promises and async/await keep asynchronous code flat.

## 📥 PDF

The complete guide is available in the PDFs below:

- **[What is a Callback Function (English).pdf](./What_is_a_Callback_Function.pdf)**
- **[What is a Callback Function (Persian).pdf](./What_is_a_Callback_Function_FA.pdf)**
