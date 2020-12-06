# Constructor Functions

# Objectives

By the end of this chapter, you should be able to:

- Explain what constructor functions are and why they are used
- Create constructor functions with proper syntax and convention
- Use the new keyword to create objects from a constructor function
- Use call and apply inside of a constructor function

# The meaning / purpose of a constructor function

Let's imagine that we are tasked with building an application that requires us to create car objects. Each `car` that we create should have a make, model and year. So we get started by doing something like this:

```js
var car1 = {
    make: "Honda",
    model: "Accord",
    year: 2002
}
var car2 = {
    make: "Mazda",
    model: "6",
    year: 2008
}
var car3 = {
    make: "BMW",
    model: "7 Series",
    year: 2012
}
var car4 = {
    make: "Tesla",
    model: "Model X",
    year: 2016
}
```

But notice how much duplication is going on! All of these objects look the same, yet we are repeating ourselves over and over again. It would be really nice to have a `blueprint` that we could work off of to reduce the amount of code that we have. That "blueprint" is exactly what a constructor function provides! Constructor functions are the closest thing we have to classes in JavaScript.

# Our first constructor function

So what is a constructor function? It's written just like any other function, except that by convention we capitalize the name of the function to denote that it is a constructor. We call these functions constructors because their job is to construct objects. Here is what a constructor function to create car objects might look like. Notice the capitalization of the name of the function; this is a **best** practice when creating constructor functions so that other people know what kind of function it is.

```js
function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
}
```

So how do constructor functions actually "construct" these objects? Through the `new` keyword that we saw before. To construct a new `Car`, use `new`:

```js
var probe = new Car('Ford', 'Probe', 1993);
var cmax = new Car('Ford', 'C-Max', 2014);

probe.make;  // Returns "Ford"
cmax.year;   // Returns 2014
```

Let's quickly refresh our memory about what the new keyword does.

# What does the `new` keyword do?

When `new` is used, the following happens:

- An empty object is created,
- The keyword `this` inside of the constructor function refers to the empty object that was just created,
- A `return this `is added to the constructor function (this is why you don't need to explicitly return any value),
- An internal link is created between the object and the `.prototype` property on the constructor function. We can actually access this link on the object that is created: it is called `__proto__`, sometimes pronounced "dunder" (double underscore) proto.

```js
function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
}

var car = new Car("Buatti", "Chiron", 2017);
car.__proto__ === Car.prototype // true
```

# The constructor property

Every single `.prototype` object has a property called `constructor` that points back to the original function. Let's look at an example:

```js
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

Person.prototype.constructor === Person // true
```

We'll explore prototypes in more detail in the next section.

# Classes

Many other programming languages have a concept of classes. In languages like Python or Java there is an explicit way of creating classes which are then used to create an instance of the class. Therefore, a class is like a blueprint for how to build something, and the instance is a construction of that blueprint. If we have a Car class, we have only one class, but there may be many instances of cars. For example, a car class can make an instance of a Ford Probe or an instance of a Bugatti Chiron. Both are instances of a car, but there is only one car class (or blueprint).

In JavaScript we **DO NOT** have classes built into the language. Instead, as a JavaScript programmer we mimic object oriented programming and classes using JavaScript constructor functions and the new keyword.

# Using call with constructors

In JavaScript, there is no way to make a traditional "class". Similarly, in JavaScript, there is no explicit way for one constructor function to inherit from another.

Instead, JavaScript has prototypal inheritance. To borrow the functionality from one constructor and use it in another, we would use the call method. Below is an example. Notice that the `Motorcycle` constructor function has an additional property that `Vehicle` does not. Conceptually, the `Motorcycle` "class" is inheriting from the `Vehicle` "class".

```js
function Vehicle(make,model,year){
    this.make = make;
    this.model = model;
    this.year = year;
}

function Motorcycle(make,model,year,motorcycleType){
    Vehicle.call(this,make,model,year)
    this.motorcycleType = motorcycleType;
}

var moto = new Motorcycle("Kawasaki", "Ninja 500", 2006, "Sports")
```

# Exercise

## Constructors Exercise

Answer the following questions and make the tests pass.

1. What is the purpose of a constructor function? 

2. What does the `new` keyword do?

3. What does the keyword `this` refer to inside of a constructor function? 

4. What is a `class`? What is an `instance`?

5. Create a constructor function for a Person, each person should have a firstName, lastName, favoriteColor and favoriteNumber.

6. Write a method called multiplyFavoriteNumber that takes in a number and returns the product of the number and the Person's favorite number

7. Refactor the following code so that there is no duplication inside the `Child` function.

```javascript
function Parent(firstName, lastName, favoriteColor, favoriteFood){
    this.firstName = firstName;
    this.lastName = lastName;
    this.favoriteColor = favoriteColor;
    this.favoriteFood = favoriteFood;
}

function Child(firstName, lastName, favoriteColor, favoriteFood){
    this.firstName = firstName;
    this.lastName = lastName;
    this.favoriteColor = favoriteColor;
    this.favoriteFood = favoriteFood;
}
```

# Next

When you're ready, move on to [Prototypes](./08-prototypes.md)