# Concepts List

1. Execution Context & Scope
2. Closures
3. this Keyword
4. Asynchronous JavaScript
5. Advanced Object Concepts
6. Event Handling & Delegation
7. Modules
8.  Memory & Performance
9.  Meta-programming
10. Browser & JS Runtime Specific

---

## Execution Context & Scope

* Global scope: available everywhere.
* Local scope: only available inside the function/block.

```js
var globalVar = "I am Global";  // Global Scope

function test() {
  var localVar = "I am Local";  // Local Scope
  console.log(globalVar);  //  Accessible
  console.log(localVar);   //  Accessible inside
}

test();

console.log(globalVar);  //  Accessible
console.log(localVar);   //  ReferenceError
```


* Lexical Scope: Functions can access variables defined in their parent scope.
 ``` js
 function outer() {
  let a = 10;

  function inner() {
    console.log(a); // inner has access to outer's variables
  }

  inner();
}

outer();

 ```


* Scope Chain: When accessing a variable, JS looks in:
    1. Current scope
    2. Parent scope/ Lexical Scope
    3. Global scope
    4. If not found → ReferenceError

---

## Closure and decorator.

**Clouser**:` function + scope `= closure — the returned inner function carries a reference to the variables it uses.
```js
function createCounter() {
  let count = 0;
  return {
    inc() { count += 1; return count; },
    get() { return count; }
  };
}
const c = createCounter();
console.log(c.inc()); // 1
console.log(c.inc()); // 2
console.log(c.get()); // 2
// `count` is private — only accessible via the methods (closures).
```

**Decorator**: A decorator is a function that wraps another function, class, or method to extend or alter its behavior without modifying its original code.
```js
function logExecution(fn) {
  return function(...args) {
    console.log(`Calling ${fn.name} with`, args);
    const result = fn(...args);
    console.log(`Result:`, result);
    return result;
  };
}

function add(a, b) {
  return a + b;
}

const loggedAdd = logExecution(add);
loggedAdd(2, 3);
```

---

## this Keyword

### Binding rules

1. **Default binding**: If no explicit object is calling the function, this falls back to window (or global) — unless strict mode, where it becomes undefined.
  ```js
  function foo() {
    console.log(this);
  }

  // Non-strict mode
  foo(); // window (in browsers) / global (in Node)

  // Strict mode
  "use strict";
  foo(); // undefined

  ```

### Implicit binding

When a function is called as a property of an object, this refers to that object.

```js
const obj = {
  value: 42,
  show() {
    console.log(this.value);
  }
};

obj.show(); // 42
```


### Explicit binding (call, apply, bind)

You can manually set this

```js
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const user = { name: "Alice" };

greet.call(user, "Hello");   // Hello, Alice
greet.apply(user, ["Hi"]);   // Hi, Alice

const bound = greet.bind(user);
bound("Hey");                // Hey, Alice
```

### Arrow functions (lexical this)
Arrow functions don’t have their own this.
They capture this from the surrounding lexical scope.

```js
const obj = {
  value: 100,
  regularFn: function () {
    console.log(this.value); // 100
  },
  arrowFn: () => {
    console.log(this.value); // undefined (or window.value)
  }
};

obj.regularFn(); // 100
obj.arrowFn();   // undefined

--------

function Timer() {
  this.seconds = 0;
  setInterval(() => {
    this.seconds++;
    console.log(this.seconds);
  }, 1000);
}

new Timer();
// Works because arrow captures `this` from Timer’s constructor

```

## Asynchronous JS

JavaScript runs in a single thread. If you block it (e.g., reading a big file or making a network call synchronously), the UI freezes or server requests stop responding.

So JS uses asynchronous patterns to let long operations run “in the background” while the main thread stays free.



### Callbacks

The old-school way: pass a function to be executed later.

```js
function fetchData(callback) {
  setTimeout(() => {
    callback(null, "data from server");
  }, 1000);
}

fetchData((err, result) => {
  if (err) console.error(err);
  else console.log(result);
});
```

**Problems:**
1. “Callback hell”
2. Error handling is messy.

### Promises

A Promise represents a value that will be available later (resolved) or an error (rejected).

```js
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve("done!"), 1000);
});

p.then(result => console.log(result))   // success
 .catch(err => console.error(err))      // error
 .finally(() => console.log("Always runs"));

---
// Promise chaining

function step1(x) {
  return Promise.resolve(x + 1);
}
function step2(x) {
  return Promise.resolve(x * 2);
}

step1(5)
  .then(step2)
  .then(result => console.log(result)); // (5+1)*2 = 12

---
// Promise error handling
Promise.resolve(42)
  .then(() => { throw new Error("Oops"); })
  .then(() => console.log("never runs"))
  .catch(err => console.error("Caught:", err.message))
  .finally(() => console.log("Cleanup"));

```

### async / await

Syntactic sugar over promises.

```js
async function run() {
  try {
    const res1 = await step1(5);
    const res2 = await step2(res1);
    console.log(res2);
  } catch (e) {
    console.error("Error:", e);
  }
}
run();

```


### Promise methods

1. `Promise.all` 

Waits for all promises → fails fast if any reject.

```js
Promise.all([Promise.resolve(1), Promise.resolve(2)])
  .then(values => console.log(values)); // [1,2]
```


2. `Promise.allSettled`
Waits for all, never fails → gives {status, value|reason}.

```js
Promise.allSettled([
  Promise.resolve("ok"),
  Promise.reject("fail")
]).then(console.log);
// [{status: "fulfilled", value: "ok"}, {status: "rejected", reason: "fail"}]

```


3. `Promise.race`

First to settle (resolve OR reject).

```js 
Promise.race([
  new Promise(res => setTimeout(() => res("fast"), 100)),
  new Promise(res => setTimeout(() => res("slow"), 500))
]).then(console.log); // "fast"

```

4. `Promise.any`

First to resolve (ignores rejections).

```js
Promise.any([
  Promise.reject("err1"),
  Promise.resolve("success"),
  Promise.reject("err2")
]).then(console.log); // "success"

```


### Microtasks vs Macrotasks

+ **Macrotasks**: setTimeout, setInterval, DOM events, I/O.
+ **Microtasks**: Promise.then, async/await (resolved promises), queueMicrotask.

> Rule: After executing each macrotask, JS clears all microtasks before moving to the next macrotask.

### Call Stack + Event Loop Working

```js 
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");

```

##### Execution order:

1. `console.log("1")` → stack
2. `setTimeout(...)` → moves to Web API → task queue
3. `Promise.then(...)` → moves to microtask queue
4. `console.log("4")` → stack
5. Stack empty → run microtasks → log "3"
6. Next macrotask → log "2"

``` terminal
1
4
3
2

```


## Advanced Object Concepts

### Object Descriptors

Every property in JS objects has descriptors:
1. value: the actual value.
2. writable: can you change it?
3. enumerable: does it show up in loops (for...in, Object.keys)?
4. configurable: can you delete it or redefine it?

```js
const user = { name: "Alice" };

// Inspect property descriptor
console.log(Object.getOwnPropertyDescriptor(user, "name"));
// {
//   value: "Alice",
//   writable: true,
//   enumerable: true,
//   configurable: true
// }

// Define property with custom descriptors
Object.defineProperty(user, "age", {
  value: 25,
  writable: false,
  enumerable: false,
  configurable: false
});

user.age = 30; //  won’t change
console.log(user.age); // 25

console.log(Object.keys(user)); // ["name"] (age not enumerable)
delete user.age; //  won’t delete


```

### Object.freeze

Makes object completely immutable (no add/remove/change).

```js

const frozen = Object.freeze({ a: 1 });
frozen.a = 2; // ❌ ignored
frozen.b = 3; // ❌ ignored
console.log(frozen); // { a: 1 }


```

### Object.seal

Can’t add/remove properties, but can modify existing values.

```js
const sealed = Object.seal({ a: 1 });
sealed.a = 2; // ✅ works
sealed.b = 3; // ❌ ignored
delete sealed.a; // ❌ ignored
console.log(sealed); // { a: 2 }

```

### Object.preventExtensions

Can’t add new properties, but can delete or change existing.

```js 
const obj = Object.preventExtensions({ a: 1 });
obj.a = 2; // ✅
obj.b = 3; // ❌ ignored
delete obj.a; // ✅
console.log(obj); // {}

```


### Spread vs Rest Operators

+ **Spread (...)** → expands arrays/objects.
+ **Rest (...)** → collects remaining elements into array/object.

```js
// Spread with arrays
const arr = [1, 2, 3];
const arr2 = [...arr, 4, 5];
console.log(arr2); // [1,2,3,4,5]

// Spread with objects
const user = { name: "Alice", age: 25 };
const user2 = { ...user, city: "NY" };
console.log(user2); // { name: "Alice", age: 25, city: "NY" }

// Rest in function params
function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3, 4)); // 10

// Rest in destructuring
const { name, ...rest } = user2;
console.log(name); // Alice
console.log(rest); // { age: 25, city: "NY" }

```

### Destructuring

```js
// Array Destructuring
const arr = [10, 20, 30];
const [first, second] = arr;
console.log(first, second); // 10 20

const [a, , c] = arr; // skip
console.log(a, c); // 10 30

const [x, ...others] = arr;
console.log(x, others); // 10 [20, 30]


---

// Object Destructuring

const user = { id: 1, name: "Charlie", age: 28 };

const { name, age } = user;
console.log(name, age); // Charlie 28

// Rename variables
const { name: username } = user;
console.log(username); // Charlie

// Default values
const { country = "Unknown" } = user;
console.log(country); // Unknown


```

