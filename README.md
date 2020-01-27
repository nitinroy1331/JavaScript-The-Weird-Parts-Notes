# JavaScript: The Weird Parts

Below is a list of notes made for the course JavaScript: The Weird Parts.

## Table of Contents
- [JavaScript: The Weird Parts](#javascript-the-weird-parts)
  * [01. Execution Contexts and Lexical Environments](#01-execution-contexts-and-lexical-environments)
    + [Syntax Parser](#syntax-parser)
    + [Lexical Environment](#lexical-environment)
    + [Execution Context](#execution-context)
    + [Name/Value Pair](#namevalue-pair)
    + [Object](#object)
    + [Hoisting](#hoisting)
    + [Phases of execution context inside JavaScript](#phases-of-execution-context-inside-javascript)
    + [JavaScript is single threaded, synchronous execution](#javascript-is-single-threaded-synchronous-execution)
    + [Invocation](#invocation)
    + [Execution Stack](#execution-stack)
    + [Variable environment](#variable-environment)
    + [Scope Chain](#scope-chain)
    + [Event Queue](#event-queue)
  * [02. Types and Operators](#02-types-and-operators)
    + [Dynamic Typing](#dynamic-typing)
    + [Primitive Type](#primitive-type)
    + [Operator](#operator)
    + [Operator Precedence](#operator-precedence)
    + [Associativity](#associativity)
    + [Coercion](#coercion)
    + [Short Circuiting](#short-circuiting)
    + [Frameworks](#frameworks)
  * [03. Objects and Functions](#03-objects-and-functions)
    + [Namespace](#namespace)
    + [First Class Functions](#first-class-functions)
    + [Expression vs Statement](#expression-vs-statement)
    + [Pass-by-value and pass-by-reference](#pass-by-value-and-pass-by-reference)
    + [Objects, Functions, and 'this'](#objects-functions-and-this)
    + ['arguments' and REST](#arguments-and-rest)
    + [Immediately Invoked Function Expressions (IIFE)s](#immediately-invoked-function-expressions-iifes)
    + [Closures](#closures)
    + [Function Factories](#function-factories)
    + [Callback Functions and Closures](#callback-functions-and-closures)
    + [Call, Apply, and Bind](#call-apply-and-bind)
    + [Functional Programming](#functional-programming)
  * [04. Object-Oriented JavaScript and Prototypal Inheritance](#04-object-oriented-javascript-and-prototypal-inheritance)
    + [Prototypal Inheritance](#prototypal-inheritance)
    + [Reflection and Extend](#reflection-and-extend)
  * [05. Building Objects](#05-building-objects)
    + [Function Constructors, 'new', and The History of JavaScript](#function-constructors-new-and-the-history-of-javascript)
    + [Function Constructors and '.prototypes'](#function-constructors-and-prototypes)
    + ['new' and Functions](#new-and-functions)
    + [Built-in Function Constructors](#built-in-function-constructors)
    + [Arrays and for ... in](#arrays-and-for--in)
    + [Object.create and Pure Prototypal Inheritance](#objectcreate-and-pure-prototypal-inheritance)
    + [ES6 and Classes](#es6-and-classes)

## 1.1 Execution Contexts and Lexical Environments
### Syntax Parser
- A program that reads your code and determines what it does, and if the
grammar/syntax is valid.
-When you write JavaScript it isn't directly telling computer what to do. You're writing code, but then someone else built programs that convert your JS into something that computer can understand. Those programs are called compilers or interpreters.**Compilers** go character by character and translate your code into a set of instructions. Important to understand is that during that process programmers who wrote that compiler can choose to do extra stuff. Your code is not what actually is given to a computer but a translation of it.

### Lexical Environment
- Where something sits physically in the code you write, the positioning, where
is it contained and what surrounds it. In JavaScript where you write something is important.

### Execution Context wrapper to help manage the code that is running.
-There are lots of lexical environments, areas of the code that you look at physically, but which one is currently running is managed via execution contexts.
- Execution context contains your running code, but it can also contain things beyond what you've written in your code. Because remember that your code is being translated by a syntax parser.
- The environment of the code being executed.
- Basically the scope of the code.
- An example:
```JavaScript
// global context

function a() {
  // a() itself is a execution context, and the global context is also an
  // execution context
  var myVar = 5;
}
```
## 1.2 Conceptual Aside - Name-Value Pairs and Objects
### Name/Value Pair
- A name which maps to a unique value, can be defined more than once, but can
have only one value in any given execution context

### Object
- A collection of Name/Value pairs, a function also counts as a value
- If the value is a primitive, it's called a property.
- If the value is a function, it's called a method.
## 3.The Global Environment and The Global Object
**Global execution context** - the base execution context. It creates global object and special variable called `this`. In a browser `this` global object is `window`.

### Hoisting
- Memory space is allocated for functions and variables, but variables are not
yet defined. All values in JS are set initially to **undefined**.
- Avoid using hoisting. Always declared methods and variables before using them because of the way JS works, you can write in your code a call to a function before an actual function and it will execute without any errors. 

```javascript
b();

function b() {
	console.log('This works!');
}
```

When parser runs through code it recognizes where you created variables and functions and it sets up memory space for them. So before your code begins to be executed, JS engine has already set aside memory space for all the variables and functions you created. When the code begins to execute line by line it can access it.

However, for variables JS engine puts a placeholder `undefined`, because it doesn't know what it's value will ultimately end up being until it starts executing that line of
code.
 
```javascript
console.log(a);

var a = 'Hello World!';
```

### Phases of execution context inside JavaScript
1. Creation of variables and functions in memory
2. Execution phase

 Conceptual Aside - Javascript and undefined
**`undefined`** - is a special value, a keyword that JavaScript sets up to all variables during a creation phase of execution context.

### JavaScript is single threaded, synchronous execution
**Single threaded**
- - JavaScript is a single threaded, synchronous language. That means one command execution at a time. Maybe not under the hood of the browser but from our perspective as programmers it is single threaded.Means that it can only execute one instruction at a time in the order that it appears.
- Asynchronous events are also actually handled synchronously.
**Synchronous execution** - means one at a time and in order that it appears. 

## 1.3 Function Invocation and the Execution Stack
**Invocation** - running or calling a function by using parenthesis `()`.
### Execution Stack
-every time you invoke a function a new execution context is created for that function and is put on top of execution stack.
- When the script is run, the global execution context is created, and is
executed.
- However, if there is another function invocation, it will stop at that line
of code, and create and execute the execution context.
# - Functions, Context, and Variable Environments
**Variable environments** - where the variables live and how they relate to each other in memory. Every execution context has its own variable environment.

## 1.4 The Scope Chain

- If the variable is not found inside the execution context, it will search for
the variable inside the outer lexical environment. If it is not found inside
the inside the outer lexical environment, then it will go up the scope chain
until it finds the variable, or when it reached the global execution context.
- Note that just because your execution context is on top of another in the
scope chain, it does not mean that it the one on the bottom is necessarily its
parent.
- An example:So in this example `myVar` would actualy log `1` even though it sits inside a function which is inside another. `myVar` sits in the outer global environment so JS will go down the scope chain until it finds it.

```JavaScript
// global context

function a() {
  /*
  Function b is sitting lexically inside function a, so when it's referring to
  myVar, if it is not found inside function b, it will go to the parent
  execution context, which is positioned inside the scope chain.
  */
  function b() {
    console.log(myVar);
  }

  b();
}

var myVar = 1;
a();
```
## 1.5 Scope, ES6, and `let`
**Scope** - where a variable is available in your code. And if it's truly a new variable, or a new copy.

**`let`** - allows JS engine block scoping. During execution context that variable is still placed in memory and set to `undefined`, however, you're not allowed to use it until the line of code is run during the execution phase. So if you try to use a variable before, you'll get an error. Also, it is declared within a block. A block is usually defined by `{}` (function, if statement etc). So if you're running `let` inside a loop a new variable will be placed in memory after each iteration.


## 1.6 What About Asynchronous Callbacks
What About Asynchronous Callbacks
**Asynchronous** - executed more than one at a time. What is happening inside JS engine is synchronous. Its just the browser is putting things asynchronously in the event queue.

**Event queue** - events like click and others are placed in the queue. And this queue starts to get processed in the order it happened only when execution stack is empty.


## 2.1 Types and Operators
### Dynamic Typing
- The type of variable does not need to be defined, it is automatically
determined in runtime.
- The variables can hold different types of values, and the data stored can
even be changed after the variable is declared.

### Primitive Type
- A type of data that represents a single value.
- These are not objects as they do not contain Name-Value pairs.
- List of primitive data types:
  1. Undefined
    - ***undefined*** represents a lack of existence, you should not assign a variable to this.
  2. Null
    - ***null*** also represents a lack of existence, but you can set a variable to this.
  3. Boolean
    - ***boolean*** is either *true* or *false*
  4. Number
    - A ***number*** is a floating point number. There is only one type of floating type number inside JavaScript.
  5. String
     - A ***String*** is a sequence of characters, and single quotes and double quotes can both be used to contain strings.
    6. Symbol
      - A ***symbol*** is used in ES6

### Operator
- A **special function** that is syntactically different.. Generally operators take two parameters and return result.
- JavaScript operators are infix notations.
- Operators are actually just functions.

### Operator Precedence
 which operator function gets called first on the same line of code. Functions are called in order of precedence (higher precedence wins).


### Associativity
- Decides what order operator functions get called, be it left-to-right , or
right-to-left if they have same level of precedence
- [Mozilla Developer Network on Associativity](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Associativity)
## 21 - Conceptual Aside Coercion
- **Coercion** is converting a value from one data type to another.
- Also known as type casting.
This happens quite often in JavaScript because it's dynamically typed. For example adding a number to a string would result in number being converted to a string:
```javascript
7 + '7'// would be = '77'
 ```
### Short Circuiting
- Because logical expressions are evaluated from left to right, they are tested
for possible '**short-circuit**' evaluation using the following rules:
  - ***false*** && (anything) is short-circuit evaluated to ***false***
  - ***true*** || (anything) is short-circuit evaluated to ***true***
  - If the statement is evaluated to ***true***, the last operand will be
  executed.
- An example:

```JavaScript
3 === 3 && 'cow' && console.log('chicken');

// Chicken will be logged to the console because if the condition is true,
// the last operand will be executed.
```
- This is particularly useful when used in functions, because it allows for
default parameters.

```JavaScript
function greet(name) {
  name && console.log('Hi, ' + name + '!');
}

greet('Sam');
```
- However, you should take note that if you pass in a **falsey** value, such as
***undefined***, ***null***, or ***0***, the conditional statement will still
evaluate to false, even if you have passed something in.

```JavaScript
function plusOne(num) {
  num = num || 1;
  return num + 1;
}

// This statement will return 2, if you pass in a falsey value such as 0
plusOne(0);
```
## 2.2 - Comparison Operators
When `true` is coerced to number it is `1`.

When `false` is coerced to number it is `0`.

When `null` is coerced to number it is `0`.

When `undefined` is coerced to number it is `NaN`.

`===` is a strict comparison operator and doesn't do conversion. So it's the best practice to always use it to prevent strange bugs due to conversion.
## 2.3 - Existence and Booleans
`undefined`, `null` and `''`(empty strings) is converted to boolean `false`.

We can use that to our advantage. This pattern is used in many JS libraries and good open source code. 
Whatever is in parenthesis of if statement it is converted to a boolean. 
So this statement returns `true` if `a` is set or `false` if not.

```javascript
var a;

if (a) {
	console.log('Something is there.');
}
```
Also worth mentioning that `false || true` returns `true.` 

## 2.4 - Default Values
If you pass two values to `||` operator it will return a first one which returns `true`.

This pattern is used in many open source code:

```javascript
var name = name || "your name";
``` 

This way you can set a default parameter value in case no arguments are passed during invocation of a function.


## 2.5 - Framework Aside: Default Values
When using a few libraries to avoid overwriting vars, most libraries use this pattern:
### Frameworks
- When you include several JavaScript files inside a HTML file, the browser
does not actually create new execution contexts. Instead, the JavaScript files
must be executed in order.
```javascript
window.variableName = window.variableName || "String";
``` 

## 3 Objects and Functions
## 3.1 - Objects and the Dot
**Objects** are name value pairs that are sitting in memory and have references to other things inside them like properties and methods.

Object can have a primitive(string, boolean) and that will be called a **property**. It can have another object and it will also be a property.

Object can also contain a function and it is called a **method**.


```javascript
var person = new Object();
person['firstName'] = 'Jason';

person.address = "111 Main St.";
```

Since `firstName` doesn't exist on person object, we create it using brackets operator and give a value to it (property).
Dot is an operator that works from left to right. It allows to access or set an object's properties.

## 27 - Objects and Object Literals
Object literal syntax is more clean and preferred way to create objects.

```javascript
var Jason = { 
    firstname: 'Jason', 
    lastname: 'Baciulis',
    address: {
        street: '111 Main St.',
        city: 'New York',
        state: 'NY'
    }
};
```
### Namespace
- a container for variables and functions. Typically to keep variables and functions with the same name separate.
You can prevent name collisions by using objects to store properties:
```JavaScript

var english = {};
var spanish = {};

english.greet = 'Hello!';
spanish.greet = 'Hola!'; JSON and Object Literals
```
JSON and Object Literals
**JSON** - JavaScript object notation is inspired by JS. In JSON property names must be wrapped in double quotes.

`JSON.stringify(objectLiteral)` - converts object to JSON.

`JSON.parse()` - converts to a JavaScript object.

### First Class Functions
**First class functions** - everything you can do with other types you can do with functions. Assign them to variables, pass them around as parameters to other functions, you can create functions on the fly.

Just like any object, function object resides in memory. Though, it's a special type of object because it has all the features of a normal object but has some other special properties. It's hidden special properties:

**Name** - though it can be anonymous ant not have a name.

**Code property** - where the actual lines of code sit.

The code that you write gets placed in the special property of the function object. So it isn't like the code you write *is* a function. The function *is* an object with other properties. And the code that you write is just one of those properties that you're adding onto it. What is special about that property that it's invocable. You can say run that code and that's when execution context creation and execution happens.

It's important that you have this mental model of functions in your mind. You have to think of functions as objects whose code just happens to be one of the properties. 
There are other things that functions can have attached to it. And it can be moved around and copied just like other object.

You have to think about **functions as more than just containers of code**.
- You can do everything to them that you can do with other types.
- E.g.
  - Being assigned to a variable
  - Passed around as an argument
  - Being created on the fly
- In JavaScript, a function is actually a special type of object, hence, we can
attach additional functions, variables, and even objects to it.
- Functions also have some hidden special properties:
  - ***name***: The name is optional, the function can be anonymous.
  - ***code***: The code inside the function is actually stored inside a code
  variable, this code variable is actually invocable.
- A function is actually just an object, whose code just happens to be one of
the properties attached to the object.

### Expression vs Statement**Expression** - a unit of code that results in a value. Statements just do work, bet expressions end up creating value. That value doesn't necessarily have to be saved to a variable.

E.g. `1 + 2` is a function expression that return a value of `3`. Do you remember how we covered that `+` operator is a function?

This is regular function statement:

```javascript
function greet() {
	console.log('hi');
}
```

It is put into memory. But it doesn't return a value until a function is invocated.

This is expression:

```javascript
var anonymousGreet = function() {
	console.log('hi');
}
```

Remember functions are objects. So it creates an object on the fly and sets it equal to the variable `anonymousGreet`. So when your code runs in the execution phase, sees the first function it just says "yeah there is function" and does nothing, just keeps going. But when it sees a variable it results in a value of a function object being created.
That's why you can call function statement before it but function expression will throw an error as "undefined is not a function" because that function is not yet created.


- An ***expression*** Is a unit of code that results in a value
- A ***statement*** a unit of code that does not result in a value

### Pass-by-value and pass-by-reference
- Primitive data types are pass-by-value, whereas objects are pass-by-reference
Conceptual Aside By Value vs By Reference
**By value** (primitives):

```javascript
var a = 3; b = a;
``` 

When you set `b` equals to `a`, equals operator sees these are primitives creates a new spot in memory and makes a copy of it. `b` and `a` will be both `3` but they are copies sitting on separate spots in memory. So if I change `a = 5` it doesn't affect `b`, it is still `3`, because after making a copy these values are on their own.

**By reference** (all objects including functions):

```javascript
var c = {greetings: 'hi'};
var d;
d = c;
```
Equals operator sees there is an object so it simply points to the same spot in memory.

After changing a value of an object: `c.greetings = 'hello'` `d` would change as well.

**Mutate** means change something.

### Objects, Functions, and 'this'
- When a function is invoked, a new execution context is invoked. When the
**code** 'property' is invoked, a new execution context is created, and put on
the execution stack. That determines how the code is executed.
- An execution context has:
  1. Variable Environment
  2. Outer Environment
  3. '***this***'
- The '***this***' keyword can point to different objects, and that depends on
where the function is and where it's called.
- When you're simply invoking a function, the ***this*** keyword inside will
always point to the global object, the **Window** object.
- If you're invoking a method inside a object, the ***this*** keyword will
point to the object itself.
- However, the '***this***' keyword when used inside a function inside a method
inside an object, will point back to the global object.
`this` inside a function points to global object.

`this` inside a method points to that object from which it is called. Left of the dot rule.

But `this` inside a function which is inside a method will point to the global object.

With ES5 JavaScript using `var` you could solve this by setting inside a method `var _this = this;` which is a very common pattern.

```JavaScript
// To be updated
var c = {
  name: 'The c object',
  log: function() {
    this.name = 'Updated c object';
    console.log(this);

    var setname = function(newname) {
      this.name = newname;
    }
    setname('Updated again! The c object');
  }
}
c.log();

```

### 'arguments' and REST
 Arrays  - Collections of Anything
**Arrays** can hold a mix of anything: functions, primitives, objects.

**Arguments** are the parameters you pass to a function. JS creates a keyword of the same name which is an array-like that contains all parameters that you passed.

In ES6 we can do: `function greet(firstname, ...other)` and `other` will be an array that contains the rest of the arguments.

- When an execution context is created, JavaScript creates the variable
environment, outer environment, this, and ***arguments***.
- The ***arguments*** variable is actually an array, and it contains all the
arguments that you have passed to the function.
- The rest operator takes the arguments that aren't defined specifically, and
get wrapped into an array.
##  - Framework Aside Function Overloading
You can call one function which inside calls another function with a certain set of parameters.

##  - Conceptual Aside  - Syntax Parsers
JavaScript engine is syntax parser in the browser.

##  - Dangerous Aside Automatic Semicolon Insertion
Semicolons are optional in JS because JS engine injects them automatically.
But it is a bad practice to not put them because you want to know what code you are writing.

## - Framework Aside Whitespace
**Whitespace** - invisible characters that create space in your code: returns, tabs, space.

### Immediately Invoked Function Expressions (IIFE)s
- IIFEs are function expressions that are immediately invoked once they are
declared.
- An example:

```JavaScript
// function statement
function greet(name) {
  console.log('Hello ' + name);
}
greet('John');

// using a function expression
var greetFunc = function(name) {
  console.log('Hello ' + name);
}
greetFunc('John');

// using an IIFE
var greeting = function(name) {
  return 'Hello ' + name;
}('Adam');
console.log(greeting);

/*
Using an anonymous IIFE, by using the parentheses
This is because JavaScript treats everything inside parentheses as
expressions. This way you're telling JS that you're not creating a new
statement, but creating a new expression instead.
*/

(function(name) {
  console.log('Hello ' + name);
}('Adam'));
```
- By wrapping code in an IIFE, we can ensure that our code does not pollute
the global namespace. (Like in any other normal function)

Framework Aside IIFEs and Safe Code
**IIFE** creates a new execution context so it's safe code because it is wrapped in separate execution context and prevents variable names collision.
Wrapping code libraries is very common pattern to not clash code with the global object.
You can reference to global object by passing `window` as a parameter.

### Closures
When a function runs and completes it is removed from execution stack, but variables created and saved in that execution phase are still stored in memory and can be accessed from down the scope.

```javascript
function greet(whattosay) {

   return function(name) {
	   console.log(whattosay + ' ' + name;
   }
	
}

var sayHi = greet('Hi');
sayHi('Jason');
```

Inside the variable `sayHi` we run `greet` function which returns another function and passes string `'Hi'`. After that functions is finished and it is popped from the execution stack. But `whattosay` variable still sits saved in the memory with a value `Hi`. So when we call function `sayHi` it will go in the outer environment and look for whattosay variable and will find it and log "Hi Jason".
We describe this proccess as execution context has closed in outer variables.

- A ***closure*** is the combination of a function and the lexical environment
within which that function was declared.
- It refers to a function having access to variables that it would normally
would have access to, even though the execution context containing those
variables is now gone.
- In short, ***closures*** make sure that the scope of a method will always be
intact, regardless of when or where you invoke a function. They make sure a
function runs the way it's supposed to.
- E.g.

```JavaScript
function greet(whattosay) {
  return function(name) {
    console.log(whattosay + '' + name);
  }
}

var sayHi = greet('Hi');

/*
The sayHi method still has access to the whattosay variable, even though
the execution context of greet no longer exists. This is automatically
done by JavaScript.

*/
sayHi('Tony');
```

```JavaScript
function buildFunctions() {
  var arr = [];

  for(var i = 0; i < 3; i++) {
    arr.push(function() {
      console.log(i);
    });
  }
  return arr;
}

var fs = buildFunctions();

/*
All three methods above will log out the number 3.
Because JavaScript actually prints out the current value of i,
not the value of i at the point of time the function was pushed into the
console. This is because the console logging is done after the method
assignments, not during the creation of the functions.
*/

fs[0]();
fs[1]();
fs[2]();
```
- If you want to change the number that is logged, you could do this:

```JavaScript
function buildFunctions2() {
  var arr = [];

  for(var i = 0; i < 3; i++) {
    let j = i;
    arr.push(function() {
      console.log(j);
    });
  }
  return arr;
}

/*
This works because a new variable is actually created every time the loop runs,
hence, the variable j will be pointing to different memory locations even if
they have the same variable name.
*/

// OR YOU COULD DO THIS (If you don't have ES5 support)

function buildFunctions2() {
  var arr = [];

  for(var i = 0; i < 3; i++) {
    arr.push(
      (function(j) {
        return function() {
          console.log(j);
        }
      }(i))
    )
  }
  return arr;
}

/*
The code above simply pushes a function that returns another function. Each
of the functions has its very own execution context, and they contain different
values of j. Hence, the console is able to log different values of j.
*/

var fs2 = buildFunctions2();

fs2[0]();
fs2[1]();
fs2[2]();
```

### Function Factories
- A function factory is simply a function that generates another function, but
the function generated may vary.
- Function factories take advantage of closures, which is the ability to make
sure that the scope of a function will always be intact.

```JavaScript
function makeGreeting(language) {
  return function(firstname, lastname) {
    if (language === 'en') {
      console.log('Hello '+ firstname + ' ' + lastname);
    }

    if (language === 'es') {
      console.log('Hola '+ firstname + ' ' + lastname);
    }
  };
}

var greetEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doa');
```

### Callback Functions and Closures
- A callback function, is a function that you pass to another function, to be
run when the other function is finished.
- An example:

```JavaScript
function sayHiLater() {
  var greeting = 'Hi';

  setTimeout(function() {
    console.log(greeting);
  }, 3000);
}

sayHiLater();
```
- But this does not work:

```JavaScript
function sayHiLater(callback) {
  var greeting = 'Hi';

  setTimeout(callback, 3000);
}

sayHiLater(function() {
  console.log(greeting);
});
```
- This is because the function is actually declared outside the function, and
since it's parent execution context is not **sayHiLater**, it cannot access
the variable greeting.

### Call, Apply, and Bind
- The Function is just another special kind of object, it has the following
attributes:
  1. Name
  2. Code (Invocable)
  3. ***call()***
  4. ***apply()***
  5. ***bind()***
  **Callback function** - a function you give to another function to be run when the other function is finished.

## 46 - `call()`, `apply()`, and `bind()`
Because functions are objects, all functions have access to built-in `call()`, `bind()` and `apply()` methods.

**`bind()`** creates a copy of a function it is attached to. E.g. `name.bind(obj)` inherits `this` from the object you pass to it as a parameter. `bind()` can also bind permanent parameters to the function: `name.bind(this, 1, 2)`. So when you call `name()` it will have parameters `1` and `2` already passed.

**`call()`** invokes the function but also lets you to decide what `this` variable will be, by passing an object: `call(object, 'param', 'param2')`. Unlike bind, it executes the function instead of copying it.

**`apply()`** is almost the same as `call()` but instead you need to pass an array as a parameter: `.apply(object, [parameters])`

In practice, you can use `call()` and `apply()` to borrow methods/functions from objects and use on another object with the same property names.

**Function currying** - creating a copy of a function but with some preset parameters.
- Bind is used when you want to change the reference of this for a function
- It creates a copy of your function, and whatever object you pass to the bind
method, becomes the object that the '***this***' keyword points to.
- The ***call*** function also lets you decide what the **this** variable will
be. The first argument for ***call*** should be what the **this** variable
should refer to. The rest of the arguments passed to it are the arguments
that you will normally pass to a function.
- The difference between the ***bind*** and the ***call*** method is that the
***call*** method actually executes the method.
- The ***call*** method and the ***apply*** method perform the same actions,
but the ***call*** method accepts the arguments directly in the parentheses,
whereas the ***apply*** method accepts the arguments in an array format.
- A use for **apply** and **call** would be function borrowing.
- Function borrowing is used to reduce redundant code, by using functions from
an object on another object.
- Example of use of **bind**, **call**, and **apply**:

```JavaScript
var person = {
  firstName: 'John',
  lastName: 'Doe',
  getFullName: function() {
    var fullName = this.firstName + ' ' + this.lastName;
    return fullName;
  }
}

var logName = function(lang1, lang2) {
  console.log('Logged: '+ this.getFullName());
  console.log('Arguments: ' + lang1 + ' ' + lang2);
  console.log('-----------');
};

var logPersonName = logName.bind(person);

logPersonName('en', 'cn');

logName.call(person, 'en', 'es');

logName.apply(person, ['en', 'my']);

// You can also create a function on the fly and invoking it using apply and call

(function(lang1, lang2) {
  console.log('Logged: '+ this.getFullName());
  console.log('Arguments: ' + lang1 + ' ' + lang2);
  console.log('-----------');
}).call(person, 'en', 'ru');

// function borrowing
var person2 = {
  firstName: 'Jane',
  lastName: 'Doe'
};

person.getFullName.call(person2, 'en', 'ru');

```
- Function currying is the act of creating a copy of a function but with some
preset parameters. This is done by passing additional arguments to the bind
method.

```JavaScript
// function currying
function multiply(a, b) {
  return a*b;
}

var multiplyByTwo = multiply.bind(this, 2);

// is actually equals to
function multiply(b) {
  var a = 2;
  return a*b;
}

```

```JavaScript
function multiply(a, b, c) {
  return a * b * c;
}

function curriedMultiply(a) {
    return function (b) {
      return function (c) {
        return a * b * c;
      }
    }
}

console.log(multiply(5, 10, 3));
console.log(curriedMultiply(5)(10)(3));
```

### Functional Programming
A mapping function is a function which takes one array and outputs another array. It is very powerful and useful technique that you will see in codebases.

```javascript
function mapForEach(arr, fn) {
    
    var newArr = [];
    for (var i=0; i < arr.length; i++) {
        newArr.push(
            fn(arr[i])   
        )
    };
    
    return newArr;
}
```

You should avoid mutating/changing things. That's why it is better to return a new array than change existing one.
- Functional programming is a programming style where you think and code in
terms of functions
- Functional programming languages are languages that have first-class functions
- Here are the examples of the use of first-class functions:

```JavaScript
function mapForEach(arr, fn) {

  var newArr = [];
  for (var i=0; i < arr.length; i++) {
    newArr.push(
      fn(arr[i])  
    );
  }
  return newArr;
}

var arr1 = [1,2,3];
console.log(arr1);

var arr2 = mapForEach(arr1, function(item) {
  return item * 2;
});
console.log(arr2);

var arr3 = mapForEach(arr1, function(item) {
  return item > 2;
});
console.log(arr3);

var checkPastLimit = function(limiter, item) {
  return item > limiter;
}

// When additional arguments to the bind method,
// a new copy of the method gets created with preset parameters.
var arr4 = mapForEach(arr1, bindy(checkPastLimit, 1));
console.log(arr4);

var checkPastLimitSimplified = function(limiter) {
  return function(limiter, item) {
    return item > limiter;
  }.bind(this, limiter);
};

var arr5 = mapForEach(arr1, checkPastLimitSimplified(2));
console.log(arr5);

```
- Instead of just separating your code into functions, first class functions
allow you to pass functions to your functions, and also return functions
from your functions.
- In functional programming, your functions should not mutate data, but instead
output data from input data.

## 04. Object-Oriented JavaScript and Prototypal Inheritance
### Prototypal Inheritance
**Inheritance** - one object gets access to properties and methods of another object.

**Classical inheritance** is the way it's been done a long time, it's what Java, C# uses. It's very verbose.

**Prototypal** is much simpler, flexible, extensible, easy to understand.

## 50 - Understanding The Prototype
All objects have built-in prototype property, including functions. Each prototype can also have its own prototype.

Each object inherits properties and methods of other objects through a prototype. So if you call a property on one object and it doesn't find it there it goes the prototype chain and looks for it on a prototype.
- ***Inheritance*** is when one object gets access to the properties and
methods of another object.
- In prototypal inheritance, each object has its **\__proto__** property,
which contains a **reference** to another object, which is its prototype.
- The prototype of an object can also have a reference to its very own
prototype.
- When you're looking for a property on an object, if it doesn't find the
property on the object you're specifying, JavaScript will go down the
prototypal chain until it finds the property specified.
- All objects in JavaScript have their own prototypes, except for the base
*Object*.
- An example would be that each function has the methods **bind**, **call**,
and **apply**
on its on prototype chain.
- A code example:

```JavaScript
var getCookies = function() {
  console.log('I has the cookies');
}

// Instead of calling getCookies.__proto__.bind(this);
// We can do this:

getCookies.bind(this);

```
Everything is an Object (Or a primitive)
Functions, arrays, and objects all have their prototype that's why we say that everything is an object in JavaScript.

All objects, functions, arrays have their prototype pointing to the special object where you can access methods like `call()`, `bind()`, `push()`. 


### Reflection and Extend
- ***Reflection*** means that an **object** can look at itself, and it can list
and change its properties and methods.
*Reflection** - an object can look at itself, listing and changing its properties and methods.

`extend(obj, obj2, obj3)` takes all properties and methods of given objects and passes them to the first object.

It is not a built in feature but many libraries have it and ES6 have `extends`.

```JavaScript
var person = {
  firstname: 'Default',
  lastname: 'Default',
  getFullName: function() {
    return this.firstname + ' ' + this.lastname;
  }
};

var john = {
  firstname: 'John',
  lastname: 'Doe'
};

for (var prop in john) {
  // To check if the property is actually directly on the object
  if (john.hasOwnProperty(prop))
    console.log(prop + ': ' + john[prop]);
}

var jim = {
  getFirstName: function() {
    return firstname;
  }
};

// Using Lodash / Underscore JS
_.extend(john, jim);
```

## 05. Building Objects
### Function Constructors, 'new', and The History of JavaScript
- A ***function constructor*** is actually just a normal function, that is used
to construct objects.
- When you create a new object using a function constructor, the following
happens:
  1. The '**new**' keyword creates a new empty object.
  2. It then binds the '**this**' keyword to that of the new empty object.
  3. It binds the ***\__proto__*** property of the new object to the prototype
  property of the function constructor.
  4. It then executes the code inside the function constructor, which sets
  the properties of the object.
  5. As long as the function constructor doesn't explicitly return a value,
  JavaScript will automatically return the object that was created.

### Function Constructors and '.prototypes'
- When the function constructor is used, the prototype is already automatically
set for the object.
- Other than name and code, all functions in JavaScript have a property called
***prototype***. Unless the function is used as a function constructor,
***prototype*** will never be used. (It is only used by the **new** operator).
- The ***prototype*** property on the function is not the prototype of the
function (The prototypes of the function are actually accessed using the
**\__proto__** keyword).
- Instead, ***prototype*** is actually the prototype of any objects created
using the function constructor.
**`.prototype`** is a property that sits in every function in JavaScript but unless you use a function constructor with `new` operator it is never used.

A `.prototype` is **not** *the* prototype of a function object. It is only a prototype of objects created with a `new` keyword.

It's better to put your methods on the `prototype` to save memory space as it gets shared between all objects.
```JavaScript
function Person(firstname, lastname) {
  console.log(this);
  this.firstname = firstname;
  this.lastname = lastname;
  console.log('This function is invoked.');
}

Person.prototype.getFullName = function() {
  return this.firstname + ' ' + this.lastname;
};

// The 'new' keyword is actually an operator, which is used to actually
// create a new object, and perform the binding of 'this'
var john = new Person('John', 'Doe');
console.log(john);

var jane = new Person('Jane', 'Doe');  
```
- When you create an object using the **new** keyword, a new empty object is
created, it binds the '***this***' keyword to the new object, and it sets the
***\__proto__*** of the empty object to the ***prototype*** property of the
function constructor that is called.
- The ***prototype*** property of function constructors is where the
***\__proto__*** property of all objects created using the function
constructors point to.
- This means that you can add additional functionality to the objects later on,
all at once since you're adding functionality to the prototype of the objects.
- Properties are often setup inside the function constructors, because
properties largely vary from object to object.
- However, since objects created from the same class have mostly the same
functionality, we could put functions on the prototype of the function
constructor, and objects can refer to the function on its prototype.

### 'new' and Functions
- Function constructors are actually just regular functions.
- That means that if you forgot to use the ***new*** keyword when creating a
new object, the function is still syntactically correct, and JavaScript won't
catch the error.
- When you try to create a new object without using the ***new*** keyword, the
function constructor will return ***undefined***. Hence, an error will be
thrown when you try to access the uncreated object.
- As a rule of thumb, any function that is intended to be used as a function
constructor, should start with a capital letter.
Any function that we intend to use as a function constructor should be named with a first capital letter. This makes it easier to spot errors in case you would miss `new` keyword.

Although worth mentioning that creating objects with function constructors is going away because of new methods and ES6
### Built-in Function Constructors
- An example built-in function constructors are:
JavaScript has some built it function constructors like: `new Number(3)` and `new String('Jason')`.

These constructors look like you're creating primitives but you are not. You are creating objects.

When you use some methods like `.length` on a string, your string is boxed in a `String` object automatically to get access to all its methods.
 
Although it doesn't work like that for numbers and you would need to create `Number` object first.
It's best to avoid using built-in function constructors to create primitives because you're not creating primitives but rather objects.
All these built-in function constructors have a prototype and you can actually add your own methods to it.
```JavaScript
var a = new Number('3');

var b = new String('John');

'John'.length;
// is equal to
var c = new String('John');
c.length;
```
- We could also add new functions to existing objects in JavaScript:

```JavaScript
String.prototype.isLengthGreaterThan = function(limit) {
  // If this is called inside a string object, it would be pointing at your
  // String object. JavaScript takes care of this by going down the prototype
  // chain.
  return this.lengthh > limit;
};

console.log('John'.isLengthGreaterThan(2));

Number.prototype.isPositive = function() {
  return this > 0;
}
```
- Built-in functions constructors especially for primitive data types are
dangerous.
- When using built-in function constructors for creating primitives, actual
primitives are not created. Hence, when strict comparison happens a primitive
and a primitive created using a built-in function constructor, the two values
will not be the same.
- Moment.js is useful for manipulating dates in JavaScript

### Arrays and for ... in
- An array in JavaScript is actually an object.
- Whenever you're adding additional functionality to a built-in functionality
constructor, when you use for ... in, the additional functionality
gets triggered even if it was not explicitly called.
- This is because the for ... in gets every property in the object, regardless
if it is directly on the object itself, or on the prototype of the object.
For arrays use standard `for` loop or `forEach`, but don't use `for in`. Because arrays are objects with `for in` you could iterate into a prototype.
```JavaScript
Array.prototype.myCustomFeature = 'cool';

var arr = ['John', 'Jane', 'Jim'];

// avoid this
for (var prop in arr) {
  console.log(prop + ': ' + arr[prop]);
}

//use this instead
for (var i=0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

### Object.create and Pure Prototypal Inheritance
- Object.create is a newer way to create objects, using pure prototypal
inheritance concepts.
- With Object.create, you create a new empty object, and the object that you
have passed in becomes your prototype.
- If you do not use '***this***' to refer to properties inside the object,
JavaScript will not be able to locate the property that you want, since the
creation of the object does not actually create a new execution context.
- With this pattern, you simply override whatever you need to by simply adding
the properties or methods to your created object.
- A ***polyfill*** is code that adds a feature which the engine *may* lack.

```JavaScript
// polyfill
if(!Object.create) {
  Object.create = function (o) {
    if (arguments.length > 1) {
      throw new Error('Object.create implementation'
      + ' only accepts the first parameter.');
    }
    function F() {};
      F.prototype = o;
      return new F();
  };
}

var person = {
  firstname: 'Default',
  lastname: 'Default',
  greet: function() {
    return 'Hi ' + this.firstname;
  }
};

// Object.create is just used to create others of person
var john = Object.create(person);
john.firstname = 'John';
john.lastname = 'Doe';
console.log(john);
```


### ES6 and Classes
- Classes in JavaScript are actually objects. When you create a new object from
a class in JavaScript, you're actually creating an object from an object.
- The ***extends*** keyword in JavaScript is actually just used to set the
prototype of an object to another object.
- ***Syntactic Sugar*** is just a different way to type something that doesn't
change how it works under the hood.
## 60 - ES6 and Classes
JavaScript has classes in ES6. However, it is not like a `class` in other languages. In other languages `class` is like a template. `class` in JS is an object by itself that you use to create other objects.

`class` doesn't change anything how objects and prototypes work under the hood. It just gives you a different way to type. Because of it you may hear JavaScript classes being called syntactic sugar.

**Syntactic sugar** - a different way to type something that doesn't change how it works under the hood.

## 61 - Initialization
Large arrays of objects are useful for testing and initialization before you have an actual data to pull from, like a JSON file.

## 62 - `typeof`, `instanceof`, and Figuring Out What Something Is
**`typeof`** is an operator (essentially a function) that excepts a parameter and returns a string.

**`instanceof`** will tell you if it has something in its prototype chain by returning a boolean.

```javascript
var a = 3;
console.log(typeof a); // returns a string 'number'

var b = "Hello";
console.log(typeof b); // returns a string 'string'

var c = {};
console.log(typeof c); // returns a string 'object'

var d = [];
console.log(typeof d); // also returns a string 'object', weird!

console.log(Object.prototype.toString.call(d)); // this little trick returns a string '[object Array]'

function Person(name) {
    this.name = name;
}
var e = new Person('Jane');
console.log(typeof e); // also an object

console.log(e instanceof Person); // returns true because Person is down the prototype chain of e

console.log(typeof undefined); // returns undefined, makes sense
console.log(typeof null); // returns an 'object', a bug since, like, forever... 

var z = function() { };
console.log(typeof z); // returns a 'function'
```

## 63 - Strict Mode
JavaScript is a more liberal of what it allows.

**`"use strict";`** - enforces more strict rules. E.g. in this mode you must declare var first to use it. In not strict mode if you forget to type `var`, it will still be created on the global object `window`.

You can use use strict at the top of the document or at the top of a function to use strict only inside it's execution context.

## 64 - Learning From Other's Good Code
There are many aspects of improving as a developer. One of the most powerful is learning from other's good code. There is a fantastic treasure trove of good code out there to learn from. Tony calls it "an open source education".

It may sound as fun as reading an encyclopedia, but you don't need to spend hours reading the source code. Find some area that's interesting to you. Don't get intimidated by famous libraries and what may seems complex patterns. Look at the structure, see what you could take away and imitate.

This is a great way to learn advanced patterns and concepts in JavaScript. So make a practice to occasionally look at the source code of the library or framework you're using.

## 65 - Deep Dive into Source Code jQuery - Part 1
When we're reading code we are not trying to understand how every feature is implemented. First, we'll try to see if we can read the code and learn how it is structured. And if we can learn some techniques and borrow some ideas.

## 66 - Deep Dive into Source Code jQuery - Part 2
jQuery has some good code you could borrow for your own projects. It has been developed and watched by many developers so it has some of the best methods and practices.

Inside jQuery there is Sizzle CSS Selector library for handling selectors.

## 67 - Deep Dive into Source Code jQuery - Part 3
**Method chaining** - calling one method after another, and each method affects the parent object.

So `obj.method1().method2()` where both methods end up with `this` variable pointing at `obj`.

## 68 - Requirements
First of all, before building any application let's think of the requirements. What this app/library should do?

## 73 - Good Commenting
Remember to write good comments for your code. Because even if you are the sole developer for a project you may need to come back after a year and you'll have to figure it out how everything works like it was someone else's code.

## 76 - TypeScript, ES6, and Transpiled Languages
**Transpile** - convert the syntax of one programming language, to another.

In this case, languages that don't really ever run anywhere, but instead are processed by transpilers that generate JavaScript.

**TypeScript** - one of the most popular transpiled languages and is created by Microsoft. The biggest difference that it uses strict types for its variables instead of dynamic types like JavaScript.
