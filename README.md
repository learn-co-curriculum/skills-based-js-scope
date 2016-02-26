# JS Scope

## Objectives
+ Explain how scope works in JavaScript
+ Explain what lexical or function-level scope is
+ Explain what block-level scope is
+ Explain how local and global variables work in JS

## What Is Scope?

Scope is the current context of your code, and can be locally or globally defined. Understanding scope is the key to understanding how your variables interact with each other in your code. Without a solid understanding of scope, your variables could be storing entirely different values than you think they are.

## Ruby versus JavaScript Scope

When talking about scope in this README, we mean variable scope — what's available in the context of your currently executing code. In Ruby, there are four levels of variable scope — global, class, instance, and local — determined by prefixes on the variable name:

- `$` for global variables
- `@@` for class variables
- `@` for instance variables
- no prefix for local variables

We'll focus on local variables for now. In Ruby, a variable's locality is determined by the function context it appears in. So in this example

```ruby
def locality
  here = true
end
```

`here` is local to the method `locality`. Similarly,

```ruby
["bread", "wine", "cheese"].each do |food|
  puts food
end
```

in the above example, `food` is local to the `each` block.

In Ruby, you can only access local variables that have been declared before the currently executing line in the current scope:

```ruby
x = 1
$x = 3
@@x = 4
@x = 5

def my_method
  y = 2
  puts $x
  puts @@x
  puts @x
  puts x
end

my_method

puts y

puts x
```

The method call `my_method` returns the unknown local variable or method error, because `x` is not defined in the scope of that method. However, it successfully prints `$x`, `@@x`, and `@x` because of their broader scopes. (You'll probably see `warning: class variable access from toplevel` for `@@x` — this is expected, and you shouldn't use class variables in this way. We're bending the rules here to highlight how local scope works.)

`puts y` returns the exact same error as `my_method` because `y` was only defined inside the method, not in the global scope. `puts x` does work, because we're printing out `x` in the same scope in which it was created.

But translate that same code to JavaScript and you get a very different result:

```js

var x = 1;

function myFunction(){
  var y = 2;
  console.log(x);
}


myFunction();
// prints out 1

console.log(y)
// error - y is not defined

console.log(x)
// prints out 1
```

The function call `myFunction();` actually does work perfectly, even `console.log(x);` This is because the variable `x` is defined in the global scope. Any variable defined in an outer scope is available to inner scopes. (So _everything_ defined in the global scope is available _everywhere_.)

However, anything defined in a function is not accessible outside of it, which is why the `console.log(y);` does not work. `y` was only defined inside the `myFunction` function, making it inaccessible anywhere else.

And again `console.log(x);` works because were printing it out in the scope in which it was defined.

## The Key To Scope

The key to scope in JavaScript boils down to the `var` keyword, which defines the scope of a variable. We reviewed this in the variables lesson, but the `var` keyword makes a variable have a local scope.

Contrast this to Ruby's variable scoping mechanisms: Where Ruby uses prefixes to determine a variable's scope, JavaScript gives you two options `var` for local variables or nothing for globals.

In our `myFunction` function, the variable `y` was defined with the `var` keyword. If instead, we had done:

```js
function myFunction(){
  y = 2;
  console.log(x);
}
```

We would be able to call `console.log(y)` outside the function as well.

Let's take our whole code example:

```js
var x = 1;

function myFunction(){
  y = 2;
  console.log(x);
}

console.log(y);

console.log(x);
```


If you copy and paste this into the JS Console, you'll get an error with `console.log(y);` At this point in time, even though `y` is a global variable, the variable hasn't been defined. JavaScript has just stored a function called `myFunction`.

If we were to do this:

```js
var x = 1;

function myFunction(){
  y = 2;
  console.log(x);
}

myFunction();

console.log(y);

console.log(x);
```
Now, because of the `myFunction` call, the global variable `y` exists, and
`console.log(y)` will work!

## The Scope of Scope, or Getting Closure

One of the most powerful things about scope in JavaScript is how easily we can
hide variables from the global scope but still make them available in inner
scopes.

```js
function outerFunction() {
  var innerVariable = "I'm sort of a secret.";

  return function innerScope() {
    var inaccessible = "Nothing can touch me.";

    return innerVariable;
  }
}
```

JavaScript has first-class functions, meaning that we can pass them around with
ease. When we call `outerFunction()`, the returned value is another function.
Let's give it a try:

```js
var myScope = outerFunction();

// returns the stringified version of `innerScope()`
myScope;
```

Cool! What happens if we try to call `innerScope()` directly?

```js
innerScope();
```

This will throw the error `undefined is not a function` (very helpful,
JavaScript). But if we call `myScope` (the variable to which we assigned the
output of `outerFunction()`):

```js
myScope();
```

We should see `"I'm sort of a secret."` returned! What happened?

Inside `outerFunction()`, we made the variable `innerVariable` available to
`innerScope()` in what we call a **closure**. In this example, `innerScope()`
_remembers_ the environment it was created in, and it maintains references to
variables declared in that environment in a closure. As we noted above,
JavaScript functions have access to their entire outer scope, so the
`innerScope()` function has access to `outerFunction()`'s environment and
to the global (`window`) environment.

Note, though, that `scopeMask()` doesn't know anything about what's in
`innerScope()` — the variable `inaccessible` is aptly named, because we can't
get at its value except inside `innerScope()`.

## A Final Note (Extra Credit)

We've over-simplified the case for JavaScript for the moment. ECMAScript 6 (ES6) introduces two new declarations, `let` and `const`. Scope-wise, the difference is that `let` and `const` have block-level scope whereas `var` only has function-level scope. This means that `let` and `const` inside, for example, an `if` block will restrict the use of a variable to that `if` block. You can try out the following at https://babeljs.io/repl

```js
if (true) {
  const blockVariable = "block";
  var localVariable = "local";
}

// you should see:
// // fizz
// // blockVariable is not defined
// on the right-hand side of the lower middle part of your screen.
console.log(localVariable);
console.log(blockVariable);
```

Try changing `const` to `let` — you should get the same error output.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/js-scope-readme' title='JS Scope'>JS Scope</a> on Learn.co and start learning to code for free.</p>

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/js-scope-readme'>Js Scope</a> on Learn.co and start learning to code for free.</p>
