# Inheritance

# Objectives:

By the end of this chapter, you should be able to:

- Define inheritance and list the benefits of using it
- Implement prototypal inheritance in JavaScript and compare and contrast different approaches to inheritance

# Prototype Inheritance

Looking at our earlier example again, we can see how our `Automobile` constructor function inherits properties from the `Object` constructor function:

```js
function Automobile(make, model, year) {
    this.make = make;
    this.model = model;
    if (year !== undefined) {
        this.year = year;
    }
}

Automobile.prototype.year = 2016;
var probe = new Automobile("Ford", "Probe", 1993);

probe.hasOwnProperty("year"); // returns true

probe.year; // returns 1993
```

Where did the function `hasOwnProperty` come from? It is defined in the `Object` prototype. Since all objects in JavaScript inherit from the Object prototype, your `Automobile` object has access to the `hasOwnProperty` function through the prototype from `Object`.

Let's investigate the prototype chain for automobile using `__proto__` and `console.dir`:

```js
var probe = new Automobile("Ford", "Probe", 1993);

// Inspect the returned object in the console
// It shows us the prototype associated with the instance of Automobile
// You should see the constructor function and a property for year
probe.__proto__;

// Inspect the returned object in the terminal
// It shows us the parent prototype (Object's prototype) that is associated
// with the instance of Automobile
// You should see many properties here, including hasOwnProperty!
probe.__proto__.__proto__;

// Click through the returned object to see the __proto__ chain.
console.dir(probe);
```

# Creating Your Own Inheritance Chain

An important concept in object oriented programming is inheritance. The idea behind inheritance is that one or more parent / super classes can pass along functions and properties to other child / sub classes.

```js
function Parent(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

Parent.prototype.sayHi = function(){
    return this.firstName + " " + this.lastName + " says hi!";
}

function Child(firstName, lastName){
    // This is how we "inherit" properties from the parent
    Parent.apply(this,arguments);
}

// This is how we inherit functions
// (create a new prototype + reset the constructor)
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;

var c = new Child("Bran", "Stark");

c.sayHi() // Bran Stark says hi!
```

So what have we done here? We've set the prototype of the `Child` to be a newly created object with a prototype of `Parent.prototype` (`Object.create` accepts as a parameter another object to set as the `prototype`).

# What does Object.create do?

Why can't we just do `Child.prototype = Parent.prototype`? Remember that when we assign objects equal to each other, they are just references. This means that Child.prototype is a reference to `Parent.prototype` which means that if we add to the Child.prototype, objects created from the `Parent.prototype` will have access to them, which would be bad!

```js
Child.prototype = Parent.prototype;

// true - this is BAD!
Child.prototype === Parent.prototype;

Child.prototype = Object.create(Parent.prototype);

// false - This is GOOD! We want these to be different
Child.prototype === Parent.prototype;
```

# What about resetting the constructor?

Let's examine the last line: `Child.prototype.constructor = Child;`. Without this line, if you examine `Child.prototype.constructor`, this will refer to the `Parent`, and not the `Child`! In many cases this won't actually matter, but it can definitely be confusing, since if you call `.prototype.constructor` on a constructor function, you expect it to point back to the original constructor function. The details here aren't that important for right now, but if you are interested in learning more, check out this Stack Overflow article.

# BAD PRACTICE: Using `new` to create a child class

You may see inheritance done by using the `new` keyword instead of using `Object.create`. This will do almost the same thing, but add additional unnecessary properties on the prototype (since it is creating an object with undefined properties just for the prototype). For more on this, check out [this](https://stackoverflow.com/questions/13040684/javascript-inheritance-object-create-vs-new) Stack Overflow question.

