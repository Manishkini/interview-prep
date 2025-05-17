# Javascript Topic 1 Closures

## I. Core Concepts

### 1. What is a closure in JavaScript?


🔑 Key Points:
1. Concept
2. Scope Access
3. Real-World Analogy

✅ Answer:
A **closure** is a function that retains access to its **lexical scope**, even when that function is executed outside of its original scope.

In simpler terms, when a function is defined inside another function, and the inner function uses variables from the outer function, a closure is created. This means the inner function remembers the variables from the outer function even after the outer function has finished executing.

Closures allow for **data encapsulation**, like creating private variables. For example:

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

Here, `counter()` keeps access to `count` even though `outer()` has already returned. That's closure in action.

This is widely used in event handlers, modules, and factory functions where **persistent private state** is needed.


---

### 2. How are closures used in real projects?


🔑 Key Points:
1. Encapsulation
2. State Preservation
3. Real Application Use

✅ Answer:
Closures are commonly used in real-world applications for **state management** and **data hiding**.

Example: Debounce utility function

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

Here, `timer` is preserved across function calls using closure. It helps prevent unnecessary re-execution of expensive operations like API calls during rapid input events.

Other examples include:
- Private variables in modules
- Custom hooks in React (useState, useCallback)
- Functional factory patterns


---

### 3. Can you create a closure example?


🔑 Key Points:
1. Code Snippet
2. Behavior Demonstration
3. Explanation

✅ Answer:
Sure. Here's a basic counter closure example:

```js
function createCounter() {
  let count = 0;
  return {
    increment: () => ++count,
    decrement: () => --count,
    getValue: () => count
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.decrement()); // 1
console.log(counter.getValue());  // 1
```

In this code:
- `count` is a private variable.
- `increment`, `decrement`, and `getValue` are closures that access `count`.

This pattern is useful in creating **modular reusable logic** with private internal states.


---

# Javascript Topic 2 Hoisting

## I. Core Concepts

### 4. What is hoisting in JavaScript?


🔑 Key Points:
1. Concept
2. Declarations vs. Initializations
3. Variable & Function Hoisting

✅ Answer:
**Hoisting** is JavaScript's behavior of moving declarations to the top of the current scope (either function or global scope) before code execution.

This means:
- **Variable declarations** (`var`, `let`, `const`) are hoisted.
- **Function declarations** are hoisted completely (including their body).
- **Initializations are not hoisted**.

Example:

```js
console.log(x); // undefined
var x = 5;
```

This is interpreted internally as:

```js
var x;
console.log(x); // undefined
x = 5;
```

But for `let` and `const`:

```js
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
```

This is due to the **temporal dead zone (TDZ)** — the time between entering scope and actual declaration.

Functions:

```js
sayHi(); // Works fine
function sayHi() {
  console.log("Hello");
}
```

Only works because `function` declarations are hoisted fully.

But:

```js
sayBye(); // TypeError: sayBye is not a function
var sayBye = function () {
  console.log("Bye");
};
```

Because function expressions are treated like variables: only the declaration (`sayBye`) is hoisted, not the function definition.


---

### 5. Are `let` and `const` hoisted too?


🔑 Key Points:
1. Yes, but...
2. Temporal Dead Zone (TDZ)
3. Safer scoping

✅ Answer:
Yes, both `let` and `const` are **technically hoisted**, but **not initialized**.

This creates a **Temporal Dead Zone (TDZ)** — a phase where the variable is in scope but not yet declared, so accessing it throws a **ReferenceError**.

Example:

```js
console.log(a); // ReferenceError
let a = 20;
```

Because of TDZ, you must **declare and initialize** `let` and `const` before usage.

They also offer **block-level scoping**, unlike `var` which is function-scoped.


---

### 6. What gets hoisted: declarations or initializations?


🔑 Key Points:
1. Declarations only
2. Not values
3. Function vs Variable

✅ Answer:
**Only declarations** get hoisted — **not initializations**.

- For `var`: variable is hoisted and initialized as `undefined`.
- For `let`/`const`: variable is hoisted but uninitialized (in TDZ).
- For `function` declarations: both the name and body are hoisted.
- For `function` expressions: only the variable name is hoisted.

Example:

```js
console.log(x); // undefined
var x = 10;

console.log(y); // ReferenceError
let y = 20;

foo(); // Hello
function foo() { console.log("Hello"); }

bar(); // TypeError
var bar = function () { console.log("Bye"); };
```

Understanding what exactly gets hoisted helps avoid bugs and improves your debugging skills.


----

# Javascript Topic 3 Scope

## I. Core Concepts

### 7. Difference between `var`, `let`, and `const`?


🔑 Key Points:
1. Scope
2. Reassignment
3. Hoisting & TDZ

✅ Answer:
| Feature        | `var`            | `let`                | `const`             |
|----------------|------------------|----------------------|---------------------|
| Scope          | Function-scoped  | Block-scoped         | Block-scoped        |
| Reassignable   | ✅ Yes           | ✅ Yes              | ❌ No               |
| Redeclarable   | ✅ Yes           | ❌ No               | ❌ No               |
| Hoisted        | ✅ Yes (as `undefined`) | ✅ Yes (TDZ) | ✅ Yes (TDZ) |
| Initial value  | `undefined`      | Not initialized      | Not initialized     |

Example:

```js
if (true) {
  var a = 1;
  let b = 2;
  const c = 3;
}
console.log(a); // 1
console.log(b); // ReferenceError
console.log(c); // ReferenceError
```

- `var` leaks outside the block due to function scope.
- `let` and `const` are confined to the block.


---

### 8. What is lexical scope?


🔑 Key Points:
1. Scope Defined at Write-time
2. Nested Functions
3. Compile-time Resolution

✅ Answer:
**Lexical scope** means that the scope of a variable is determined by its **position in the source code**.

Inner functions have access to variables defined in **outer functions**, even after the outer function has returned — this also forms the basis of **closures**.

Example:

```js
function outer() {
  const name = "Manish";
  function inner() {
    console.log(name); // Accessible due to lexical scope
  }
  return inner;
}

const greet = outer();
greet(); // "Manish"
```

- Here, `inner` has access to `name` because of where it was **written**, not how it's called.


---

### 9. What is block scope vs function scope?


🔑 Key Points:
1. `let` / `const` = Block Scoped
2. `var` = Function Scoped
3. Scoping Affects Lifetime & Access

✅ Answer:
**Block Scope** (introduced in ES6): Variables declared with `let` and `const` exist **only within the nearest set of curly braces `{}`**.

```js
{
  let x = 10;
  const y = 20;
}
console.log(x); // ReferenceError
```

**Function Scope**: `var` variables are scoped to the **entire function body**, not blocks.

```js
function demo() {
  if (true) {
    var test = "inside";
  }
  console.log(test); // "inside"
}
```

Understanding the difference helps prevent **variable leakage** and bugs due to premature access.


---

# Javascript Topic 4 Data Types And Equality

## I. Core Concepts

### 10. What are the primitive types in JavaScript?


🔑 Key Points:
1. 7 Primitive Types
2. Immutable
3. Stored by Value

✅ Answer:
JavaScript has **7 primitive data types**, which are:

1. `String` – e.g., `'hello'`
2. `Number` – e.g., `42`, `3.14`
3. `Boolean` – `true` or `false`
4. `Null` – explicitly empty
5. `Undefined` – declared but not assigned
6. `Symbol` – unique and immutable value (ES6)
7. `BigInt` – large integers beyond safe `Number` limits (ES11)

**Primitive types are immutable** and stored **by value**, meaning changes to one copy don't affect another.

```js
let a = "hi";
let b = a;
b = "bye";
console.log(a); // "hi"
```

Unlike **objects**, primitives do not share references.


---

### 11. Difference between `==` and `===`?


🔑 Key Points:
1. `==` does type coercion
2. `===` is strict comparison
3. Prefer `===` always

✅ Answer:
- `==` compares values **after type coercion**
- `===` compares both **value and type** (strict)

Examples:

```js
0 == false        // true
0 === false       // false

'5' == 5          // true
'5' === 5         // false

null == undefined // true
null === undefined // false
```

**Best Practice:** Always use `===` to avoid bugs caused by implicit conversions.

Use `==` only if you **know the coercion behavior** and it’s intentional.


---

### 12. What is `typeof null` and why is it an object?


🔑 Key Points:
1. Historical Bug
2. `typeof null` === 'object'
3. Check with strict equality

✅ Answer:
```js
typeof null; // 'object'
```

This is a **historical bug** in JavaScript. In the original implementation, values were tagged using the lower bits of their type representation — `null` was tagged as an object and this hasn't been fixed for backward compatibility.

If you want to check if a variable is actually `null`, **don't use `typeof`**. Instead:

```js
x === null; // true
```

To safely check for null and undefined:

```js
if (x == null) {
  // catches both null and undefined
}
```

Remember: `typeof null` being `'object'` is a **gotcha** — know it to avoid surprise bugs.


---

# Javascript Topic 5 Objects And Prototypes

## I. Core Concepts

### 13. What is prototypal inheritance?


🔑 Key Points:
1. Object Inheritance
2. Shared Properties via Prototype Chain
3. Not Class-Based (Historically)

✅ Answer:
**Prototypal inheritance** means that objects in JavaScript can inherit properties and methods from other objects via a special internal link called `[[Prototype]]`.

In modern JS, this is accessed via `__proto__` or `Object.getPrototypeOf()`.

Example:

```js
const parent = { greet() { return "hello"; } };
const child = Object.create(parent);

console.log(child.greet()); // "hello"
console.log(child.__proto__ === parent); // true
```

Here, `child` inherits the `greet()` method from `parent`.

Unlike class-based languages (Java, C++), JavaScript's inheritance is **object-to-object**, allowing **flexible and dynamic inheritance**.


---

### 14. What is the prototype chain?


🔑 Key Points:
1. Lookup Path
2. Ends at `null`
3. Enables Reuse

✅ Answer:
The **prototype chain** is how JavaScript resolves properties and methods.

When accessing a property, JavaScript:
- First looks at the object itself.
- If not found, it looks at the object's prototype.
- And so on, until it hits `null`.

Example:

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHello = function () {
  return "Hi, " + this.name;
};

const user = new Person("Manish");
console.log(user.sayHello()); // "Hi, Manish"
```

If `sayHello` wasn't found on `user`, JavaScript looks in `Person.prototype`, forming a **chain**.

Eventually:
```
user → Person.prototype → Object.prototype → null
```

This is called the **prototype chain**.


---

### 15. How does `Object.create()` work?


🔑 Key Points:
1. Creates a New Object
2. Sets Prototype
3. Alternative to `new`

✅ Answer:
`Object.create(proto)` creates a **new object with the specified prototype**.

Example:

```js
const animal = {
  speak() {
    return "Generic sound";
  }
};

const dog = Object.create(animal);
dog.bark = function () {
  return "Woof!";
};

console.log(dog.speak()); // "Generic sound"
console.log(dog.bark());  // "Woof!"
```

This approach is often used in **pure prototypal inheritance** and **object composition** patterns.

You can also pass a second argument to define properties:

```js
const obj = Object.create(Object.prototype, {
  key: { value: "value", writable: true }
});
```

This method gives **fine-grained control over inheritance**.


---

# Javascript Topic 6 This Keyword

## I. Core Concepts

### 16. How does `this` behave in different contexts?


🔑 Key Points:
1. Depends on call-site
2. Different in strict mode
3. Arrow functions don't bind `this`

✅ Answer:
The value of `this` depends on **how a function is called**, not where it’s defined.

#### 🔹 Global Context
```js
console.log(this); // In browser: `window`, In Node: `{}`
```

#### 🔹 Inside a Regular Function
```js
function show() {
  console.log(this);
}
show(); // In browser: `window`, In strict mode: `undefined`
```

#### 🔹 As an Object Method
```js
const user = {
  name: "Manish",
  greet() {
    console.log(this.name);
  }
};
user.greet(); // "Manish"
```

#### 🔹 With `call`, `apply`, or `bind`
```js
function greet() {
  console.log(this.name);
}
const person = { name: "Kini" };
greet.call(person); // "Kini"
```

#### 🔹 In Arrow Functions (Lexical `this`)
```js
const obj = {
  name: "Manish",
  greet: () => {
    console.log(this.name);
  }
};
obj.greet(); // `this` refers to outer scope, not `obj`
```

🔁 Arrow functions do **not bind their own `this`** — they inherit it from the surrounding context (lexical scope).


---

### 17. What’s the difference between `call`, `apply`, and `bind`?


🔑 Key Points:
1. All set the value of `this`
2. `call` and `apply` invoke the function
3. `bind` returns a new function

✅ Answer:

| Method | Purpose | Syntax | Execution |
|--------|---------|--------|-----------|
| `call` | Invoke function with custom `this` | `fn.call(thisArg, arg1, arg2)` | Immediately |
| `apply` | Same as `call`, but args in array | `fn.apply(thisArg, [args])` | Immediately |
| `bind` | Returns a new function with bound `this` | `const boundFn = fn.bind(thisArg)` | Later |

#### Example:

```js
function greet(greeting) {
  return `${greeting}, ${this.name}`;
}
const person = { name: "Kini" };

console.log(greet.call(person, "Hello")); // "Hello, Kini"
console.log(greet.apply(person, ["Hi"])); // "Hi, Kini"

const sayHey = greet.bind(person);
console.log(sayHey("Hey")); // "Hey, Kini"
```

Use `bind` when you want to **preserve `this`** for future calls, like in event handlers.


---

### 18. What is lexical `this` in arrow functions?


🔑 Key Points:
1. Arrow functions don’t have their own `this`
2. `this` is captured from the enclosing scope
3. Useful in callbacks and class methods

✅ Answer:
Arrow functions **do not bind their own `this`**. Instead, they inherit it **lexically** — from the **scope where they're defined**.

Example:

```js
function Timer() {
  this.seconds = 0;
  setInterval(() => {
    this.seconds++;
    console.log("Arrow Count:", this.seconds);
  }, 1000);
}

new Timer(); // ✅ Increments every second correctly
```

If we used a regular function in `setInterval`, `this` would be `undefined` or refer to `window`, **not the instance**.

This lexical behavior is especially useful in:
- React components
- Event handlers
- Timeout/callback functions


---

# Javascript Topic 7 Functions

## I. Core Concepts

### 19. What is the difference between function declaration and expression?


🔑 Key Points:
1. Declaration is hoisted
2. Expression is not hoisted (assigned at runtime)
3. Syntax difference

✅ Answer:

#### 📌 Function Declaration
```js
function greet() {
  return "Hello!";
}
```
✅ Can be called **before it's defined**, because it’s **hoisted**.

#### 📌 Function Expression
```js
const greet = function () {
  return "Hello!";
};
```
❌ Cannot be called before declaration — only the variable `greet` is hoisted, not its value.

```js
sayHi(); // ❌ TypeError: sayHi is not a function
var sayHi = function () {
  console.log("Hi");
};
```

Use function expressions when you:
- Want to assign a function to a variable
- Need closure over scoped variables
- Use functions as arguments


---

### 20. What is a higher-order function?


🔑 Key Points:
1. Accepts functions as arguments
2. Returns a function
3. Enables abstraction

✅ Answer:
A **higher-order function (HOF)** is a function that:
- **Takes another function as an argument**
- OR **returns a function**

Examples:

#### 📌 Takes a function:
```js
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}
repeat(3, console.log); // Logs 0, 1, 2
```

#### 📌 Returns a function:
```js
function multiplier(factor) {
  return function (number) {
    return number * factor;
  };
}
const double = multiplier(2);
console.log(double(5)); // 10
```

This is foundational for:
- Functional programming
- React hooks (`useCallback`, `useMemo`)
- Array methods like `.map`, `.filter`, `.reduce`

---

### 21. What is a pure function?

🔑 Key Points:
1. Same input = same output
2. No side effects
3. Easier to test

✅ Answer:
A **pure function** is a function that:
- **Always returns the same result** given the same inputs
- **Has no side effects** (doesn’t modify external variables or state)

```js
function add(a, b) {
  return a + b;
}
```

This is pure. But:

```js
let total = 0;
function impureAdd(a) {
  total += a;
}
```

This is **impure** because it modifies `total`, an external variable.

Pure functions are easier to:
- **Test**
- **Debug**
- **Compose**

They are central to **functional programming** and libraries like Redux.

---

# Javascript Topic 8 Event Loop And Async

## II. Asynchronous JavaScript

### 22. How does the JavaScript event loop work?

🔑 Key Points:
1. Single-threaded
2. Task queues (macro + micro)
3. Non-blocking async execution

✅ Answer:
JavaScript uses an **event loop** to handle asynchronous operations like `setTimeout`, `fetch`, and `promises` — even though it's single-threaded.

#### 🧠 What happens behind the scenes?

1. **Call Stack** — keeps track of currently executing functions.
2. **Web APIs** — browser APIs like `setTimeout`, `fetch`.
3. **Task Queues**:
   - **Microtask Queue** — Promises, `queueMicrotask`
   - **Macrotask Queue** — `setTimeout`, `setInterval`, UI events

#### 🔄 Event Loop
- When the call stack is empty, the **event loop** pushes tasks from the queue.
- **Microtasks are processed first**, then macrotasks.

#### Example:
```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

✅ Output:
```
Start
End
Promise
Timeout
```

➡️ Because promises go to the **microtask queue**, and timeouts go to the **macrotask queue**.

---

### 23. What are microtasks and macrotasks?

🔑 Key Points:
1. Microtasks: Promises, queueMicrotask
2. Macrotasks: setTimeout, setInterval, fetch
3. Microtasks run before macrotasks

✅ Answer:
- **Microtasks**: run **after the current operation**, before the next rendering or macrotask
  - e.g., `Promise.then`, `queueMicrotask`
- **Macrotasks**: queued by browser timers or IO events
  - e.g., `setTimeout`, `setInterval`, `setImmediate`

#### Example:
```js
console.log("A");

setTimeout(() => console.log("B"), 0);
queueMicrotask(() => console.log("C"));
console.log("D");
```

✅ Output:
```
A
D
C
B
```

---

### 24. Explain the output of this async/await + setTimeout example

```js
async function test() {
  console.log("1");
  await Promise.resolve();
  console.log("2");
}
test();
setTimeout(() => console.log("3"), 0);
console.log("4");
```

🔑 Key Points:
1. `await` yields to microtask queue
2. `setTimeout` is macrotask
3. Execution is order-sensitive

✅ Answer:
Output:
```
1
4
2
3
```

#### Why?
- `console.log("1")` runs immediately.
- `await` pauses the function and schedules the continuation (`console.log("2")`) as a **microtask**.
- `setTimeout(..., 0)` goes to **macrotask** queue.
- `console.log("4")` runs next from the current stack.
- Then the **microtask** (`2`) runs.
- Then the **macrotask** (`3`) runs.

Understanding this sequencing is crucial for debugging async bugs and race conditions.

---

# Javascript Topic 9 Promises

## II. Asynchronous JavaScript

### 25. How does `async/await` work under the hood? Do they use generator functions?

🔑 Key Points:
1. Syntactic sugar over Promises
2. Internally modeled like generator functions
3. Managed by async function state machine

✅ Answer:
Yes — `async/await` is **syntactic sugar over Promises**, and **conceptually inspired** by **generator functions**.
```

### 🔧 How it works (Step-by-step):

```js
async function example() {
  console.log("Start");
  await Promise.resolve("Done");
  console.log("End");
}
example();
```

```js
// Equivalent with generator:
function* exampleGen() {
  console.log("Start");
  yield Promise.resolve("Done");
  console.log("End");
}
const gen = exampleGen();
gen.next().value.then(() => gen.next()); // Simulated async flow

### 26. What is a promise in JavaScript?

🔑 Key Points:
1. Represents a future value
2. Has 3 states
3. Avoids callback hell

✅ Answer:
A **Promise** is a JavaScript object that represents the eventual **completion or failure** of an asynchronous operation.

It has **three states**:
- `pending`
- `fulfilled`
- `rejected`

Example:
```js
const promise = new Promise((resolve) => {
  setTimeout(() => resolve("Done!"), 1000);
});

promise.then(console.log); // "Done!" after 1 second
```


### 27. How is `.then()` different from `async/await`?


🔑 Key Points:
1. `then()` = callback style
2. `await` = cleaner syntax
3. Both are based on Promises

✅ Answer:
- `.then()` handles promise chaining using callbacks
- `async/await` uses a synchronous style for asynchronous code

Example:
```js
fetchData()
  .then(data => process(data))
  .then(result => display(result))
  .catch(err => handleError(err));
```

Versus:

```js
try {
  const data = await fetchData();
  const result = await process(data);
  display(result);
} catch (err) {
  handleError(err);
}
```


### 28. How to handle errors with promises?


🔑 Key Points:
1. Use `.catch()` for `.then()` chains
2. Use `try...catch` with `async/await`

✅ Answer:

```js
// With then/catch
fetch("/api")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));

// With async/await
async function getData() {
  try {
    const res = await fetch("/api");
    const data = await res.json();
    console.log(data);
  } catch (error) {
    console.error("Error:", error);
  }
}
```


### 29. Convert a callback-based function to a promise-based one


🔑 Key Points:
1. Use `new Promise()`
2. Wrap legacy callback

✅ Answer:
```js
// Callback version
function loadData(cb) {
  setTimeout(() => cb(null, "Data loaded"), 1000);
}

// Promisified version
function loadDataPromise() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("Data loaded"), 1000);
  });
}

loadDataPromise().then(console.log);
```

---

# Javascript Topic 10 Call Apply Bind

## II. Asynchronous JavaScript

### 30. What are `call()`, `apply()`, and `bind()` in JavaScript?

🔑 Key Points:
1. All three are used to **manually set `this`**
2. `call()` and `apply()` invoke the function immediately
3. `bind()` returns a new function with `this` permanently set


---

### ✅ 1. `call()`

**Definition:**
Invokes the function **immediately**, setting `this` to the first argument. Remaining arguments are passed **individually**.

```js
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const person = { name: "Manish" };
greet.call(person, "Hello"); // Hello, Manish
```

---

### ✅ 2. `apply()`

**Definition:**
Same as `call()`, but **arguments are passed as an array**.

```js
greet.apply(person, ["Hi"]); // Hi, Manish
```

✅ Use `apply()` when you already have an array of arguments (e.g., using `Math.max.apply(null, [1,2,3])`).

---

### ✅ 3. `bind()`

**Definition:**
Returns a **new function** with `this` permanently bound to the given object. **Does not execute immediately**.

```js
const sayHello = greet.bind(person, "Hey");
sayHello(); // Hey, Manish
```

✅ Use `bind()` for:
- Event handlers
- Delayed execution
- Preserving `this` in callbacks

---

### 🧠 Summary Table

| Method  | Executes? | Arguments       | Returns         |
|---------|-----------|------------------|-----------------|
| `call`  | Yes       | Individual args  | Return value    |
| `apply` | Yes       | Array of args    | Return value    |
| `bind`  | No        | Individual args  | New function    |

---

# Javascript Topic 11 Array Methods

## III. Array & Object Manipulation

### 31. What’s the difference between `map`, `filter`, and `reduce`?

```
🔑 Key Points:
1. `map` transforms each element
2. `filter` selects elements that match a condition
3. `reduce` accumulates to a single value

✅ Answer:
These are all **higher-order functions** used for manipulating arrays.
```

---

### 🔹 `map()`

- Transforms **each item** in the array
- Returns a **new array** of the same length

```js
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2); // [2, 4, 6]
```

---

### 🔹 `filter()`

- Filters items based on a **condition**
- Returns a **new array** with matching items

```js
const nums = [1, 2, 3, 4];
const evens = nums.filter(n => n % 2 === 0); // [2, 4]
```

---

### 🔹 `reduce()`

- Reduces array to a **single value** (e.g., sum, average, object)
- Takes a callback `(accumulator, currentValue) => ...`

```js
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, val) => acc + val, 0); // 10
```

#### Reduce Example for an Object:
```js
const names = ["Alice", "Bob", "Alice"];
const count = names.reduce((acc, name) => {
  acc[name] = (acc[name] || 0) + 1;
  return acc;
}, {});
// { Alice: 2, Bob: 1 }
```

---

### 🧠 Summary Table

| Method   | Purpose                  | Returns        |
|----------|--------------------------|----------------|
| `map`    | Transform array          | New array      |
| `filter` | Select based on condition| New array      |
| `reduce` | Accumulate values        | Single value   |

---

# Javascript Topic 12 Destructuring Spread Rest

## III. Array & Object Manipulation

### 32. What is object and array destructuring?

🔑 Key Points:
1. Unpack values into variables
2. Works for arrays and objects
3. Can rename or use defaults

✅ Answer:

#### 🔹 Array Destructuring:
```js
const nums = [1, 2, 3];
const [a, b] = nums;
console.log(a, b); // 1, 2
```

You can skip values:
```js
const [x, , z] = [10, 20, 30];
console.log(x, z); // 10, 30
```

#### 🔹 Object Destructuring:
```js
const user = { name: "Manish", age: 27 };
const { name, age } = user;
console.log(name, age); // "Manish", 27
```

You can also:
- Rename: `const { name: userName } = user`
- Add default: `const { email = "N/A" } = user`

---

### 33. What is the spread operator (`...`) in JavaScript?

🔑 Key Points:
1. Expands arrays/objects
2. Useful in copying, merging
3. Shallow copy

✅ Answer:
The **spread operator** (`...`) unpacks values from arrays or objects.


#### 🔹 Array spread:
```js
const a = [1, 2];
const b = [...a, 3, 4]; // [1, 2, 3, 4]
```

#### 🔹 Object spread:
```js
const obj1 = { x: 1 };
const obj2 = { ...obj1, y: 2 }; // { x: 1, y: 2 }
```

Used for:
- Cloning arrays/objects
- Merging multiple sources
- Passing arguments to functions: `Math.max(...[1, 2, 3])`

---

### 34. What is the rest parameter (`...`) in JavaScript?

🔑 Key Points:
1. Gathers remaining items
2. Opposite of spread
3. Works in function arguments and destructuring

✅ Answer:

#### 🔹 In function parameters:
```js
function sum(...args) {
  return args.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3); // 6
```

#### 🔹 In array destructuring:
```js
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest);  // [2, 3, 4]
```

#### 🔹 In object destructuring:
```js
const { a, ...others } = { a: 1, b: 2, c: 3 };
console.log(a);      // 1
console.log(others); // { b: 2, c: 3 }
```

---

# Javascript Topic 13 Rest Vs Spread

## III. Array & Object Manipulation

### 35. Difference Between Rest Parameter and Spread Operator

🔑 Key Points:
1. Both use `...` syntax
2. Purpose differs: collecting vs expanding
3. Context matters (function vs data structure)


---

### ✅ Rest Parameter (`...args`)

| Feature             | Description                                 |
|---------------------|---------------------------------------------|
| Purpose             | **Collect** remaining elements into an array |
| Used In             | **Function arguments** or **destructuring** |
| Output              | Always an **array**                         |
| Position            | Must be the **last parameter**              |

#### 📌 Example in a function:
```js
function logAll(first, ...rest) {
  console.log(first); // 1
  console.log(rest);  // [2, 3, 4]
}
logAll(1, 2, 3, 4);
```

#### 📌 Example in destructuring:
```js
const [head, ...tail] = [10, 20, 30];
console.log(head); // 10
console.log(tail); // [20, 30]
```

---

### ✅ Spread Operator (`...arr`)

| Feature             | Description                                |
|---------------------|--------------------------------------------|
| Purpose             | **Expand** iterable elements               |
| Used In             | Arrays, objects, function calls            |
| Output              | Individual elements                       |
| Position            | Can be used **anywhere** in expressions    |

#### 📌 Example in arrays:
```js
const nums = [1, 2];
const moreNums = [...nums, 3, 4]; // [1, 2, 3, 4]
```

#### 📌 Example in function calls:
```js
const args = [5, 10];
Math.max(...args); // 10
```

---

### 🧠 Summary Table

| Feature           | Rest Parameter                      | Spread Operator                   |
|-------------------|--------------------------------------|-----------------------------------|
| Syntax            | `...name` in function/destructuring | `...name` in calls/literals       |
| Function Use      | Collects args into array             | Spreads array into arguments      |
| In Arrays/Objects | Collects remaining items             | Copies/merges items               |
| Output            | Always an array                      | Individual elements               |

----

# Javascript Topic 14 Object Utils

## III. Array & Object Manipulation

### 36. Difference between `Object.keys()`, `Object.values()`, and `Object.entries()`

🔑 Key Points:
1. All used to inspect objects
2. Return different formats
3. Useful in iterations and transformations

✅ Answer:

#### 📌 `Object.keys(obj)`
Returns an array of **own property names** (keys).
```js
const user = { name: "Manish", age: 27 };
console.log(Object.keys(user)); // ["name", "age"]
```

#### 📌 `Object.values(obj)`
Returns an array of **own property values**.
```js
console.log(Object.values(user)); // ["Manish", 27]
```

#### 📌 `Object.entries(obj)`
Returns an array of **key-value pairs** as nested arrays.
```js
console.log(Object.entries(user)); 
// [["name", "Manish"], ["age", 27]]
```

Use it with destructuring:
```js
for (const [key, value] of Object.entries(user)) {
  console.log(key, value);
}
```

---

### 37. How to deep clone an object in JavaScript?

🔑 Key Points:
1. Shallow copy ≠ Deep copy
2. Use recursion or libraries for deep copy
3. JSON trick has limitations

✅ Answer:

#### ❌ Shallow copy:
```js
const obj1 = { a: 1, b: { c: 2 } };
const copy = { ...obj1 };
copy.b.c = 99;
console.log(obj1.b.c); // 99 – because it's shallow
```

#### ✅ Deep copy with JSON:
```js
const deep = JSON.parse(JSON.stringify(obj1));
```

**Limitations**:
- Loses functions
- Doesn’t handle `Date`, `Map`, `Set`, `undefined`

#### ✅ Deep copy with structuredClone (modern JS):
```js
const copy = structuredClone(obj1);
```

---

### 38. What is `Object.freeze()`?

🔑 Key Points:
1. Makes an object immutable
2. Prevents new properties or changes
3. Shallow only

✅ Answer:

```js
const user = { name: "Manish" };
Object.freeze(user);

user.name = "Kini";     // ❌ Ignored silently
user.email = "new@x.in" // ❌ Cannot add
console.log(user);      // { name: "Manish" }
```

Note: `freeze()` is **shallow** — nested objects can still be mutated:

```js
const obj = { inner: { a: 1 } };
Object.freeze(obj);
obj.inner.a = 10; // ✅ Works, because `inner` wasn't frozen
```

✅ Deep freeze requires recursion.

---

# Javascript Topic 15 Garbage Collection

## V. Miscellaneous (Advanced & Tooling)

### 39. What is garbage collection in JavaScript?

🔑 Key Points:
1. JS uses automatic garbage collection
2. Based on reachability
3. Uses reference-counting & mark-and-sweep

✅ Answer:
JavaScript automatically reclaims memory using **garbage collection**.

The most common algorithm used is **mark-and-sweep**:
- The engine finds all **reachable** objects from root (e.g., `window`, `global`)
- Anything not reachable is considered garbage and collected

```js
let a = { name: "Manish" };
a = null; // original object is now unreachable → eligible for GC
```

✅ No need to manually free memory.
🚫 But you can create **memory leaks** (e.g., global variables, uncleaned DOM references).

---

### 40. How do memory leaks happen in JS and how to prevent them?

🔑 Key Points:
1. Caused by unreferenced-but-retained objects
2. Global variables, timers, closures can leak
3. Prevent with cleanup and scoping

✅ Answer:

#### Common sources of memory leaks:
- **Global variables**: accidentally not scoped
- **Uncleared setInterval/setTimeout**
- **Detached DOM nodes still in memory**
- **Closures holding references unintentionally**

```js
let cache = {};
function saveData(key, value) {
  cache[key] = value; // over time, memory grows
}
```

✅ Prevention tips:
- Use `let/const`, avoid unintentional globals
- Clear intervals/timeouts: `clearInterval`, `clearTimeout`
- Clean up event listeners (`element.removeEventListener`)
- Use WeakMap/WeakSet for object caches (GC-friendly)

---

# Javascript Topic 16 Modules

## V. Miscellaneous (Advanced & Tooling)

### 41. Difference between CommonJS and ES Modules

🔑 Key Points:
1. CommonJS = Node.js, synchronous
2. ES Modules = modern standard, async
3. Syntax difference

✅ Answer:

| Feature         | CommonJS                | ES Modules                   |
|------------------|--------------------------|-------------------------------|
| Syntax          | `require()` / `module.exports` | `import/export`          |
| Execution       | Synchronous              | Asynchronous                 |
| Scope           | Wrapped in function      | Strict mode, block-scoped    |
| Used In         | Node.js (legacy)         | Modern JS + browsers         |

#### 📌 CommonJS:
```js
const fs = require("fs");
module.exports = myFunc;
```

#### 📌 ES Module:
```js
import fs from "fs";
export default myFunc;
```

✅ Use ES Modules for modern JS and tree-shaking support.

---

# Javascript Topic 17 Try Catch Finally

## V. Miscellaneous (Advanced & Tooling)

### 42. How does `try...catch` work in JavaScript?

🔑 Key Points:
1. Used for synchronous error handling
2. `catch` handles exceptions
3. `finally` always runs

✅ Answer:
```js
try {
  // code that might throw
  const result = riskyFunction();
  console.log(result);
} catch (error) {
  console.error("Caught error:", error.message);
} finally {
  console.log("Cleanup or always-run code");
}
```

- `try` block contains code that may throw an error
- `catch` block handles the error
- `finally` runs **no matter what** — even if an error occurs or not

---

### ❌ Important:
`try...catch` only works for **synchronous** code:

```js
try {
  setTimeout(() => {
    throw new Error("Async error"); // ❌ Won’t be caught
  }, 1000);
} catch (e) {
  console.log("Will not catch this");
}
```

✅ Use `try/catch` inside an **async function** with `await`:

```js
async function fetchData() {
  try {
    const res = await fetch("/api");
    const data = await res.json();
    return data;
  } catch (err) {
    console.error("Async error:", err);
  }
}
```

---

### 43. What is the `finally` block used for?

🔑 Key Points:
1. Always runs
2. Used for cleanup
3. Works with both success/failure

✅ Answer:
The `finally` block is executed **after the `try` and `catch`**, regardless of whether an error was thrown.

Use it for:
- Closing resources (DB, files)
- Hiding loading indicators
- Cleanup operations

Example:
```js
try {
  console.log("Task started");
  throw new Error("Oops!");
} catch (e) {
  console.log("Caught error");
} finally {
  console.log("Always runs"); // ✅
}
```

--------------------------------------------------------------------------------

# Javascript Topic 18 Optional Chaining Nullish

## V. Miscellaneous (Advanced & Tooling)

### 44. What is optional chaining (`?.`) in JavaScript?

🔑 Key Points:
1. Safely access nested properties
2. Prevents runtime errors on `undefined` or `null`
3. Stops evaluation if value is nullish

✅ Answer:
**Optional chaining (`?.`)** allows you to safely access deeply nested properties without checking every level manually.

#### 📌 Without optional chaining:
```js
const user = { profile: { name: "Manish" } };
// user.settings.theme → ❌ runtime error if `settings` is undefined

const theme = user.settings && user.settings.theme;
// ❌ messy and repetitive
```

#### ✅ With optional chaining:
```js
const theme = user.settings?.theme; // ✅ undefined (no error)
```

It short-circuits and returns `undefined` if **anything before `?.` is `null` or `undefined`**.

---

### 45. What is nullish coalescing (`??`) in JavaScript?


🔑 Key Points:
1. Provides default **only** for `null` or `undefined`
2. Better than `||` for falsy values like 0 or ""
3. Often used with `?.`

✅ Answer:
**Nullish coalescing (`??`)** returns the **right-hand value only if the left is `null` or `undefined`**.

#### 📌 Example:
```js
const username = user.name ?? "Guest";
```

- If `user.name` is `"Manish"` → `"Manish"`
- If `user.name` is `null` or `undefined` → `"Guest"`

✅ Unlike `||`, it doesn’t treat `0`, `false`, or `""` as nullish.

#### ❌ Using `||`:
```js
const count = 0 || 10; // 10 ❌ not desired
```

#### ✅ Using `??`:
```js
const count = 0 ?? 10; // 0 ✅ correct
```

---

### 🔥 Combo Example

```js
const user = {
  profile: {
    preferences: null
  }
};

const theme = user.profile?.preferences?.theme ?? "dark";
console.log(theme); // "dark" — safely accessed + fallback
```

--------------------------------------------------------------------------------

# Javascript Topic 19 Generators

## V. Miscellaneous (Advanced & Tooling)

### 46. What are generators in JavaScript?

🔑 Key Points:
1. Functions that can pause (`yield`) and resume
2. Use `function*` and `yield`
3. Return an iterator

✅ Answer:
A **generator** is a special function that can **pause execution** using `yield` and **resume later** using `.next()`.

You define a generator with `function*`, and use `yield` to return values one-by-one.

---

### 🔹 Basic Example:

```js
function* greet() {
  yield "Hi";
  yield "Hello";
  yield "Welcome";
}

const gen = greet();

console.log(gen.next()); // { value: "Hi", done: false }
console.log(gen.next()); // { value: "Hello", done: false }
console.log(gen.next()); // { value: "Welcome", done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

---

### 🔹 Iterating through a generator:

```js
for (const message of greet()) {
  console.log(message);
}
// Output:
// Hi
// Hello
// Welcome
```

---

### 🔹 Accepting input into generator:

```js
function* add() {
  const x = yield "First number?";
  const y = yield "Second number?";
  return x + y;
}

const it = add();
console.log(it.next());       // { value: "First number?", done: false }
console.log(it.next(5));      // { value: "Second number?", done: false }
console.log(it.next(10));     // { value: 15, done: true }
```

🧠 You can **send values back in** via `.next(value)`

---

### 🔹 Why use generators?

✅ Use cases:
- Custom iterators (`for...of`)
- Lazy evaluation
- Infinite sequences
- Async workflows (before `async/await`)
- Redux Saga (manages side effects in React apps)

--------------------------------------------------------------------------------

# Javascript Topic 20 Tooling Babel Polyfill

## V. Miscellaneous (Advanced & Tooling)

### 47. What is Babel and why is it used?

🔑 Key Points:
1. JavaScript compiler (transpiler)
2. Converts modern JS to backward-compatible JS
3. Enables ES6+ in old browsers

✅ Answer:
**Babel** is a **JavaScript compiler** that lets you write modern ES6+ code and convert (transpile) it into an older version (like ES5) to ensure **browser compatibility**.

#### Example:
```js
// ES6+
const greet = (name = "Guest") => `Hello, ${name}`;

// Babel output
var greet = function(name) {
  if (name === void 0) name = "Guest";
  return "Hello, " + name;
};
```

✅ Used in tools like **Webpack, Vite, Next.js** to enable:
- Arrow functions
- Async/await
- JSX (in React)

---

### 48. What is a transpiler vs a compiler?

🔑 Key Points:
1. Transpiler → same-level language (e.g., JS → older JS)
2. Compiler → higher-to-lower language (e.g., Java → bytecode)
3. Babel is a transpiler

✅ Answer:
A **transpiler** translates code **from one version of a language to another** (e.g., ES6 → ES5).

A **compiler** typically converts high-level code into **machine code** or **intermediate bytecode** (e.g., C++ → binary, Java → JVM bytecode).

Babel is a transpiler, not a traditional compiler — it doesn’t generate machine code.

---

### 49. What is tree shaking?

🔑 Key Points:
1. Removes unused code during bundling
2. Requires ES Modules (`import/export`)
3. Optimizes bundle size

✅ Answer:
**Tree shaking** is a build optimization technique that **removes unused exports** from your final JavaScript bundle.

✅ Supported by tools like **Webpack**, **Rollup**, **esbuild**, and **Vite**.

#### Example:
```js
// utils.js
export function used() { return "Used"; }
export function unused() { return "Unused"; }

// main.js
import { used } from './utils';
```

In production builds, `unused()` is **eliminated**.

🚫 Doesn’t work with `require()` (CommonJS) — only with ES Modules (`import/export`)

### 50. Bonus: What is polyfilling vs transpiling?

🔑 Key Points:
1. Polyfill = runtime shim
2. Transpile = code conversion
3. Often used together

✅ Answer:
- **Transpiling**: Convert code to older syntax (e.g., async → generators)
- **Polyfilling**: Add missing APIs that **don’t exist natively** in the browser


#### Example:
```js
if (!Array.prototype.includes) {
  Array.prototype.includes = function (val) {
    return this.indexOf(val) !== -1;
  };
}
```

✅ Babel can **transpile syntax** and inject polyfills (via `@babel/preset-env` + `core-js`).

---