---
publishDate: 2023-10-27T09:30:00Z
# author: Pantelis Theodosiou
title: Demystifying 'this' in JavaScript
excerpt: Unravel the complexities of the 'this' keyword in JavaScript, understanding its behavior in different contexts and how to effectively use it in your code.
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
# category: Tutorials
tags:
  - javascript
  - web-development
# metadata:
#   canonical: https://astrowind.vercel.app/get-started-website-with-astro-tailwind-css
---

One of the core ideas of JavaScript is the `this` keyword, which is essential for figuring out the context in which a function is run. Nonetheless, many developers may find it confusing, particularly those who are unfamiliar with the language. This piece will examine the `this` keyword, its characteristics, and practical applications for it.

## What is the "this" keyword?

Think of `this` as a magic pointer that indicates the context in which a function is operating. It's similar to having a code-cracking secret decoder ring that indicates your current location. Depending on how a function is called, `this`'s value may vary dynamically. Let's dissect it using a few examples.

### Default Binding

The global object, which is typically the browser's window object, is referred to as `this` when a function is called without any specific binding.

```js
function sayHi() {
  console.log(this);
}

sayHi(); // Logs the global object (usually 'window' in a browser)
```

In this case, `this` points to the global context because the function is called in the global scope.

### Implicit Binding

Occasionally, `this` is implicitly associated with the function's owning object. When a function is used as an object's method, this occurs:

```js
const person = {
  name: "Pantelis",
  greet: function() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

person.greet(); // Logs "Hello, my name is Pantelis"
```

Because the function is called within the context of `person`, `this` inside the `greet` method is bound to the `person` object in this instance.

### Explicit Binding

Additionally, you can use methods like `call()`, `apply()`, and `bind()` to explicitly set the value of `this`.

#### Using `call()` and `apply()`

When contacting a JavaScript function, you can set this variable's value explicitly using the `call()` and `apply()` methods. Their handling of function arguments differs slightly, but otherwise they are fairly similar.

##### `call()`

The `call()` method is used to invoke a function and explicitly specify the value of this and any additional arguments as separate parameters.

```js
function introduce(language) {
  console.log(`My name is ${this.name} and I code ${language}`);
}

const person = { name: "Pantelis" };
introduce.call(person, "JavaScript"); // Logs "My name is Pantelis and I code JavaScript"
```

In this example above, `call()` sets `this` to the `person` object and passes the argument `JavaScript` to the `introduce` function.

##### `apply()`

The `apply()` method is similar to `call()`, but it accepts an array or an array-like object as the second parameter for passing arguments.

```js
function introduce(language) {
  console.log(`My name is ${this.name} and I code ${language}`);
}

const person = { name: "Pantelis" };
introduce.apply(person, ["JavaScript"]); // Logs "My name is Pantelis and I code JavaScript"
```

`apply()` sets `this` to the `person` object and passes the argument `JavaScript` in the above example, operating similarly to `call()`.

#### Using `bind()`

`bind()` differs from `call()` and `apply()` methods. `bind()` creates a new function with the given `this` value but does not call the function right away. When you want to create a function with a fixed context for use at a later time, this can be helpful:

```js
function introduce(language) {
  console.log(`My name is ${this.name} and I code ${language}`);
}

const person = { name: "Pantelis" };
const introducePantelis = introduce.bind(person, "JavaScript");

introducePantelis(); // Logs "My name is Pantelis and I code JavaScript"
```

In this example, `bind()` creates a new function `introducePantelis`, which has a fixed `this` value set to the `person` object and a predefined argument `JavaScript`. You can call `introducePantelis()` later to execute the function with these predetermined values.

#### Use Cases

* `call()` and `apply()` are suitable for immediately invoking functions with specific `this` values and arguments,
* `bind()` is useful when you want to create a function with a predefined context and arguments for future use, often in event handlers or callbacks.

### New Binding

`this` is automatically bound to the newly created object when you use a `constructor` function with the `new` keyword:

```js
function Dog(name) {
  this.name = name;
}

const dog1 = new Dog("Rocky");
console.log(dog1.name); // Logs "Rocky"
```

`this` in this instance refers to the recently formed object within the `Dog` function.

### Arrow Functions and `this`

The way the arrow functions differs slightly. They take on the `this` value from the lexical context that surrounds them rather than having their own `this` binding:

```js
const outer = {
  inner: () => {
    console.log(this);
  }
};

outer.inner(); // Logs the global object (window in a browser)
```

Because it is defined in the global scope in this example, `this` inside the arrow function is the global object.

Gaining proficiency in JavaScript requires an understanding of the `this` keyword. You can write more efficient and maintainable code by understanding its behavior and putting the concepts of default binding, implicit binding, explicit binding, and new binding to use. 

But be aware of common pitfalls as well, such as using arrow functions when dynamic binding is required or losing the `this` context in callback functions. If you practice a lot, you'll become an expert in JavaScript `this` very soon!