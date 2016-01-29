# JS Scope

## Objectives
+ Explain how scope works in JavaScript
+ Explain what lexical or function-level scope is
+ Explain what block-level scope is
+ Explain how local and global variables work in JS

## What Is Scope?

Scope is the current context of your code, and can be locally or globally defined. Understanding scope is the key to understanding how your variables interact with each other in your code. Without a solid understanding of scope, your variables could be storing entirely different values than you think they are.

## Ruby Versus JavaScript Scope

Ruby has **block-level** scope, which means that every block (like an each block, method, and class has their own scope.) Scope can be nested. You can have a class with it's own scope, inside that class a method with another scope, and even down to an if-statement inside that method with it's own scope. 

JavaScript handles scope a little bit differently. JavaScript has **function-level** or **lexical** scope, which means that every function you define has it's own scope, but no other structures do. 

In Ruby,

```ruby
x = 1

def my_method
  y = 2
  puts x
end

my_method 

puts y

puts x
```

The method call `my_method` returns the unknown local variable or method error, because `x` is not defined in the scope of that method.

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

The function call `myFunction();` actually does work perfectly, even `console.log(x);` This is because the variable `x` is defined in the "global" scope. Any variable defined in the main scope is accessible inside any functions.

However, anything defined in a function not accessible outside, which is why the `console.log(y);` does not work. `y` was only defined inside the `myFunction` function, making it inaccessible anywhere else.

And again `console.log(x);` works because were printing it out in the scope in which it was defined.

## The Key To Scope

The key to scope in JavaScript boils down to the `var` keyword, which defines the scope of a variable. We reviewed this in the variables lesson, but the `var` keyword makes a variable have a local scope.

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


If you copy and paste this into the JS Console, you'll get an error with `console.log(y);` At this point in time, even though `y` is a global function, the variable hasn't been defined. JavaScript has just stored a function called `myFunction`.

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
Now, because of the `myFunction` call, the global variable `y` exists, and `console.log(y)` will work!


<p data-visibility='hidden'>View <a href='https://learn.co/lessons/js-scope-readme' title='JS Scope'>JS Scope</a> on Learn.co and start learning to code for free.</p>
