# JS Scope

## Objectives
+ Explain how scope works in JavaScript
+ Explain what lexical or function-level scope is
+ Explain what block-level scope is
+ Explain how local and global variables work in JS

## What Is Scope?

Scope is the current context of your code. Understanding scope is the key to understanding how your variables interact with each other in your code. Without a solid understanding of scope, your variables could be storing entirely different values than you think they are.

## The Key To Scope

To determine a variable's scope in JavaScript, ask yourself two questions:

    1. Is it declared inside a function?
    2. Is it declared with the `var` keyword?

If a variable is declared in the outermost scope of a project, it's going to be global no matter what – that is, whether you write `var foo = 'bar'` or `foo = 'bar'`. Remember, saying that a variable is "global" in JavaScript just means that it's a property of the top-most object. (In the browser, that object is `window`; in other environments (like Node.js), it could be `global`.)

Inside a function, variables declared with `var`, `const`, or `let` are only available within that function's scope. Variable's declared without one of these keywords attach themselves to the global object.

``` javascript
function myFunction() {
  const y = 2;
  console.log(y);
}
```

In `myFunction` function above, the variable `y` was defined with the `const` keyword, so it's only available inside the function. If we run `myFunction()` in console, we'll see `2` printed out. But if we try `console.log(y);` in console, we'll get an error. If instead, we write

```js
function myFunction(){
  y = 2;
  console.log(y);
}
```

We can call `console.log(y)` outside the function as well.

Let's take another example:

```js
const x = 1;

function myFunction(){
  y = 2;
  console.log(x);
}

console.log(y); // Error!

console.log(x); // 1
```

If you copy and paste this into the JS Console, you'll get an error with `console.log(y);` At this point in time, even though `y` is a global variable, the variable hasn't been defined. JavaScript has just stored a function called `myFunction`.

If we were to do this:

```js
const x = 1;

function myFunction(){
  y = 2;
  console.log(x);
}

myFunction();

console.log(y);

console.log(x);
```
Now, because of the `myFunction` call, the global variable `y` exists, and `console.log(y)` will work!

## The Scope of Scope, or Getting Closure

One of the most powerful things about scope in JavaScript is how easily we can hide variables from the global scope but still make them available in inner scopes.

```js
function outerFunction() {
  var innerVariable = "I'm sort of a secret.";

  return function innerScope() {
    var inaccessible = "Nothing can touch me.";

    return innerVariable;
  }
}
```

JavaScript has first-class functions, meaning that we can pass them around with ease. When we call `outerFunction()`, the returned value is another function.

Let's give it a try:

```js
var myScope = outerFunction();

// the stringified version of `innerScope()`
myScope;
```

Cool! What happens if we try to call `innerScope()` directly?

```js
innerScope();
```

This will throw the error `undefined is not a function` (very helpful, JavaScript). But if we call `myScope` (the variable to which we assigned the output of `outerFunction()`):

```js
myScope();
```

We should see `"I'm sort of a secret."` returned! What happened?

Inside `outerFunction()`, we made the variable `innerVariable` available to `innerScope()` in what we call a **closure**. In this example, `innerScope()` _remembers_ the environment it was created in, and it maintains references to variables declared in that environment in a closure. As we noted above, JavaScript functions have access to their entire outer scope, so the `innerScope()` function has access to `outerFunction()`'s environment and to the global (`window`) environment.

Note, though, that `outerFunction()` doesn't know anything about what's in `innerScope()` — the variable `inaccessible` is aptly named, because we can't get at its value except inside `innerScope()`.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/js-scope-readme' title='JS Scope'>JS Scope</a> on Learn.co and start learning to code for free.</p>
