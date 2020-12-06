# Call, Apply, and Bind.

# Objectives:

By the end of this chapter, you should be able to:

- Compare and contrast the parameters and return values of call, apply, and bind
- Discuss use cases for each of these methods
- Change the context of the keyword this using call, apply, and bind

# A quick review of the keyword this

In a previous section, we mentioned four ways to determine the value of the keyword `this`. 
Here's a bit more detail on each type of binding:

- Default binding - catch-all rule, when the keyword this is in the global context
- Implicit Binding - when the keyword this is attached to a parent object
- Explicit Binding - when we want to explicitly set the context for the keyword `this`
- The `new` keyword - when the `new` keyword is used, the keyword this refers to an object that is created from a function after the new keyword (usually called a constructor function).

So far, we've seen default binding and implicit binding in more detail. In this chapter we'll focus on the third way to determine the value of the keyword this: explicit binding. To explicitly bind the keyword this we can use one of three functions: `call, apply, or bind`. These functions allow us to set the value of the keyword `this` to be whatever we want!

# `call`

`call` will immediately invoke the function that it is attached to. If you want to change the value of the keyword `this`, you can pass in the desired value as the first parameter to `call` (oftentimes this first argument is written in documentation as `thisArg`). Here's an example:

```js
var animal = {
    introduce: function() {
        return this.name + " is a " + this.type + " and says " + this.sound + "!";
    }
};

var whiskey = {
    name: "Whiskey",
    type: "dog",
    sound: "woof"
};

var moxie = {
    name: "Moxie",
    type: "cat",
    sound: "meow"
};

// set the thisArg to be the object whiskey:
animal.introduce.call(whiskey); // "Whiskey is a dog and says woof!"

// set the thisArg to be the object moxie:
animal.introduce.call(moxie); // "Moxie is a cat and says meow!"
```

The second to n-th parameters of call are the parameters of the function you are immediately invoking, separated by commas. Here's an example passing multiple arguments into call:

```js
var person1 = {
    name: "Matt",
    greet: function(otherName) {
        return "Hi, " + otherName + ", I'm " + this.name + ". Nice to meet you!";
    }
}

var person2 = { 
    name: "Tim"
}

// person1 greets person2:

person1.greet(person2.name); // "Hi Tim, I'm Matt. Nice to meet you!

// person2 tries to greet person1, but there's a problem...

person2.greet(person1.name); // TypeError: person2.greet is not a function

// person2 doesn't have a greet method! So let's borrow the one from person1...

person1.greet.call(person2); // "Hi, undefined, I'm Tim. Nice to meet you!"

// We still need to pass person1's name to the function being called! Let's pass the argument to the function inside of call:

person1.greet.call(person2, person1.name); // "Hi, Matt, I'm Tim. Nice to meet you!"
```

One of the most common use cases for `call` is to convert an array-like object into an actual array. Take a look at the following example:

```js
function sumArgumentsIncorrectly(){
    return arguments.reduce(function(acc,next){
        return acc + next;
    },0);
}

sumArgumentsIncorrectly(1,2,3,4,5) // this throws a type error because the arguments "array-like object" does not contain the method reduce!

function sumArgumentsCorrectly(){
    // we are going to use the slice method which makes copies of arrays, but instead of making a copy of [], we will use the arguments array as the context that we want slice to be called in. We can immediately attach reduce and we are good to go!
    return [].slice.call(arguments).reduce(function(acc,next){
        return acc + next;
    },0)
}

sumArgumentsCorrectly(1,2,3,4,5) // 15
```

# Apply

`apply` is very similar to `call`, except it only takes in a maximum of two arguments. As with `call`, the first argument to apply is thisArg, which lets us explicitly set the value of the keyword this. Unlike `call`, however, the second argument to apply is always an array of parameters. In other words, `fn.call(thisArg,arg1,arg2,arg3)` will produce the same result as `fn.apply(thisArg,[arg1,arg2,arg3])` for any function fn, this argument thisArg, and arguments `arg1, arg2, and arg3`.

So when should you use call and when should you use apply? If you don't have to pass any additional arguments to the function on which you're invoking call or apply, it doesn't matter: you can use either one. If you do need to pass arguments to the function, do whatever is most convenient in the situation. If you have an array of parameters to pass, or you don't know exactly how many arguments you'll be passing, apply might be a better bet. If you only have one argument to pass, or you always know which arguments you'll need to pass, maybe call is a better choice.

```js
var matt = {
    firstName: "Matt",
    lastName: "Lane",
    instructor: true,
    favColor: "blue",
    dogOwner: true,
    deleteInfo: function() {
        console.log(arguments);
        for (var i = 0; i < arguments.length; i++) {
            delete this[arguments[i]];
        }
    }
}

var tim = {
    firstName: "Tim",
    lastName: "Garcia",
    instructor: true,
    favColor: "blue",
    dogOwner: false
};

var elie = {
    firstName: "Elie",
    lastName: "Schoppik",
    instructor: true,
    favColor: "purple",
    dogOwner: false
};

matt.deleteInfo('instructor', 'favColor');
matt; // {firstName: "Matt", lastName: "Lane", dogOwner: true}

matt.deleteInfo.apply(tim,['firstName', 'dogOwner', 'favColor']);
tim; // {lastName: "Garcia", instructor: true}

matt.deleteInfo.apply(elie,['instructor','favColor','dogOwner','lastName']);
elie; // {firstName: "Elie"}
```

Here are two other common cases where you'll see `apply` being used. One involves the use of the Math.max function. This function returns the maximum number in a comma-separated list of numbers, but you can also use apply to find the maximum value of an array:

```js
var numbersArray = [1,2,3,4,5];

Math.max.apply(this,numbersArray) // 5
```

Another use-case involves taking an array of arrays and flattening it, so that all the inner arrays are removed (but the values inside of them are preserved). Here's how you could do this using apply:

```js
var arrayToBeFlattened = [1,2,[3,4],[5,6]]

[].concat.apply([],arrayToBeFlattened) // [1,2,3,4,5,6]
```

# `Bind`

Just like `call`, bind `takes` in as its first parameter what we would like the context of the keyword `this` to be and the 2nd to n-th parameters are the arguments to the function. However, bind does not immediately invoke the function. Instead, it returns a function definition that can be called at a later point in time (or right away, but you will very rarely do this). In other words, `fn.bind(thisArg,arg1,arg1,arg3)() `(note the parentheses at the end!!) does the same thing as `fn.call(thisArg,arg1,arg2,arg3)`.

```js
function add(a,b){
    return a+b;
}

var partialAdd = add.bind(this,2)
partialAdd(4) // 6
```

# A review of Closure

In a previous unit we mentioned that closure is:

When a function is able to access variables from an outer function that has already returned.

We see that the bind function returns a new function to us, so what bind is doing is simply using closure! Let's take a look a reimplementation of a simpler version of bind (JavaScript's implementation is quite a bit more complex).

```js
function bind(fn,thisArg){
    var outerArgs = Array.prototype.slice.call(arguments)
    var argsWeWant = outerArgs.slice(2) // we don't want the fn and thisArg values! Let's copy from the 2nd index of the arguments array to the end!
    return function(){
        return fn.apply(thisArg, argsWeWant.concat([].slice.call(arguments))) 
        /*/
        remember that the 2nd parameter of apply takes in an array. 
        So we are concatenating (joining) the arguments from the outer  function with the arguments from the inner function 
        to form 1 big array of arguments to be used when the inner function is finally called.
        /*/
    }
}

function add(a,b){
    return a+b;
}

// check out some cool stuff we can do with our bind function now!

bind(add,this,1,11)() // 12
bind(add,this)(1,11) // 12
bind(add,this,1)(11) // 12
bind(add,this,1)(11,5,6,7,8,9) // 12 - the rest are ignored!
```

A very common use case for bind is when you don't want to lose the correct context of the keyword this, but also do not want to execute the function immediately:

```js
var obj = {
    firstName: "Elie",
    sayHi: function(){
        setTimeout(function(){
            console.log(this.firstName + " says hi!");
        }.bind(this),1000);
    }
}
```

In this example, we are ensuring that we get the correct context of the keyword this. If we did not use the `bind(this)`, the context of the keyword `this` would be the window object, because when the callback inside of the setTimeout executes, it does not do so as a method on obj, so it loses the implicit binding. In particular the window does not have firstName property!

So why did we not use `call` or `apply`? Remember that bind does not execute the function right away. Instead it just returns a function definition. For the example above that is exactly what we want, because we don't want to run the function right away: we want to wait 1000 milliseconds!

When you're ready, move on to [The 'new' Keyword](./04-new.md)

