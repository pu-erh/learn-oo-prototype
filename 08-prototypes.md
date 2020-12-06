# Prototypes

# Objectives

By the end of this chapter, you should be able to:

- Define what an object's prototype is
- Explain what effect using the new keyword has on an object's prototype
- Explain why functions should be added to the prototype instead of the constructor function
- Create a "class" in javascript that inherits from a parent "class"

# Prototype Intro

Every single function that is created in JavaScript has a `prototype` property. Moreover, each object that is created can access its constructor's prototype property via the object's own `__proto__` property.

Let's start by looking at `Object.prototype`. In the Chrome console, try typing `Object.prototype` then expand the object you get back. You can see that `Object` already has many properties on its prototype.

When you create a constructor function, that function will have it's own prototype. Let's try that out by creating a `Person` constructor function:

```js
function Person(name) {
   this.name = name;
}

var tim = new Person("Tim");

Person.prototype; // Object {}
```

So far, our `Person` constructor function has a prototype and the only two properties available on the prototype should be `constructor` and `__proto__`. Let's try adding a function to the Person prototype:

```js
Person.prototype.sayHello = function() {
    return "Hello, " + this.name;
};
```

Now that we have added `sayHello` to the prototype of `Person`, any person object that will be create or that was created in the past has access to the function:

```js
var moxie = new Person("Moxie");
moxie.sayHello();  // returns "Hello, Moxie"

// Notice that sayHello still works for tim even though tim was created
// before the sayHello function was added to the prototype.
tim.sayHello();    // returns "Hello, Tim"
```

So the main things to know so far about an Object's prototype are the following:

- Any function or property added to the prototype is shared among all instances linked to that prototype (For example, sayHello is **shared** among all Person instances).
- Each constructor function has its own prototype.

# Shared Prototype Example

Let's look at another example of adding properties to a prototype.

```js
function Person(name){
    this.name = name;
}

Person.prototype.siblings = ["Haim", "David"];

var elie = new Person("Elie");
```

The above example creates a instance of a `Person` and sets a `siblings` array on the prototype. The intention is for `elie` to have an array of siblings. However, since the prototype is shared among all instances of a `Person`, any other instance will also have access to the same siblings array:

```js
elie.siblings.push("Tamar"); // returns the new length of the array => 3
// The siblings array will now be ["Haim", "David", "Tamar"]

var anotherPerson = new Person("Mary");

anotherPerson.siblings.push("Leslie");
elie.siblings; // ["Haim", "David", "Tamar", "Leslie"]
```

We can see again from this example, that anything put on the prototype is **shared** among all instances of that object.

# Constructor Function Best Practices

The best practices for creating constructor functions in JavaScript are:

- All of the properties that you do not want to be shared go inside of the constructor function
- All properties that you want to be shared go on the prototype. Almost all of the time, you will want to put functions on the prototype. We will explain why soon!

Using our person example, if we want to add a siblings array to the `Person` class, we would add it in the constructor:

```js
function Person(name) {
    this.name = name;
    this.siblings = [];
}

var janey = new Person("Janey");
janey.silbings.push("Annie");
```

Now each time the `new` keyword is used on the `Person` constructor, a new object is created that has its own name and siblings property. Now if we create another person it will have its own name and siblings array as well:

```js
var tim = new Person("Tim");
tim.siblings.push("Nicole");
tim.siblings.push("Jeff");
tim.siblings.push("Greg");
tim.siblings; // Returns ["Nicole", "Jeff", "Greg"];
```

We said earlier that when it comes to functions, you typically want to add them to the prototype. Why is this? After all, your code will function correctly if you create your function definitions in the constructor like this:

```js
// NOT A GOOD PRACTICE
function Person(name) {
  this.name = name;
  this.sayHi = function() {
    return "Hello, " + this.name;
  }
}
```

The problem is that every time you use the `new` keyword to create a `Person`, a new object gets created in memory that allocates space for the person's name and also for the `sayHi` function. So if we have 10 `Person` objects that we create, there will be 10 copies of the same `sayHi` function. Since the function does not need to be unique per `Person` instance, it is better to add the function to the prototype, like this:

```js
// BEST PRACTICE!!
function Person(name) {
   this.name = name;
}

Person.prototype.sayHi = function() {
    return "Hello, " + this.name;
}
```

Unless you have a good reason not to, **always put function definitions on the constructor function's prototype**.

# JavaScript Property Lookup

When you attempt to access a property on an object in JavaScript, there is a lookup process that goes on in order to find your property. To find the value for a property, first the properties on the object are checked. If the property is not found, then the properties on the prototype of the constructor function are checked. Let's look at an example:

```js
function Automobile(make, model, year) {
    this.make = make;
    this.model = model;
    if (year !== undefined) {
        this.year = year;
    }
}

Automobile.prototype.year = 2016;
```

Notice that year is set on the prototype to 2016. Also, if no year is passed into the constructor, an assignment to year will not be made.

```js
var newCar = new Automobile("Ferrari", "488 Spider");

// In this case, we did not pass in a year,
// so it was never set in the constructor function
newCar.hasOwnProperty("year"); // Returns false

newCar.year; // returns 2016
```

Now, if we create a car with a year, the property on the car object will be seen first in the property lookup:

```js
var probe = new Automobile("Ford", "Probe", 1993);

probe.hasOwnProperty("year"); // returns true

probe.year; // returns 1993
```

# Exercise

## Prototypes Exercise 

### Part I:

- Create a constructor function for a Person. Each person should have a firstName, lastName, favoriteColor, favoriteNumber and favoriteFoods (which should be an array).

- Add a function on the Person.prototype called `fullName` that returns the firstName and lastName property of an object created by the Person constructor concatenated together.

```javascript
var p = new Person("Shana", "Malarkin", "Green", 38);
p.fullName(); // Shana Malarkin
```

- Overwrite the `toString` method from the Object prototype by creating a `toString` method for `Person`.  The `toString` method should return a string in the following format:


```javascript
var p = new Person("Shana", "Malarkin", "Green", 38);
p.toString(); // Shana Malarkin, Favorite Color: Green, Favorite Number: 38
```

- Add a property on the Person object called `family` which is an empty array.

- Add a function on the Person.prototype called `addToFamily` which adds an object constructed from the Person constructor to the `family` array. To make sure that the object you are adding is an object construced from the Person constructor - take a look at the `instanceof`operator. Make sure that your `family` array does not include duplicates! This method should return the length of the `family` array.

```javascript
var p = new Person("Shana", "Malarkin", "Green", 38) 
p.family; // []
p.addToFamily(p); // 1
p.family.length; // 1
p.addToFamily(p); // 1 - No duplicates!
p.family.length; // Length should still be 1
```

### Part II:

- Implement your own version of `Array.prototype.map`
- Implement a function that reverses a string and place it on the `String.prototype`
- Implement your own version of `Function.prototype.bind`

### Part III:

For the last part, let's think less about the actual code we need to write and more about thinking in an Object Oriented way. 

- Let's imagine that we are building an application which allows users to play chess. What constructor functions would we need? What kinds of prototype functions and properties would we need as well?

- Let's imagine that we are building a game of Tic Tac Toe. What kinds of prototype functions and properties would we need as well?

# Next

When you're ready, move on to [Inheritance](./09-inheritance.md)