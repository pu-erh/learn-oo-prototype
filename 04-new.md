# The 'new' Keyword

# Objectives:

By the end of this chapter, you should be able to:

- Explain what the new keyword does and when it is used
- Revisit the four ways the keyword this is determined

# The `new` keyword

So far we have seen three ways for the value of the keyword this to be determined. We have seen the default (or global) binding, implicit (or object) binding, and explicit binding (with call, apply, and bind). Now let's explore the last way the keyword `this` can be determined: with the new keyword.

Let's take a look at some code:

```js
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}
```

The first thing you might notice here is that the name of the function is capitalized. This will not effect the function in any way, but is a signal to ourselves and other developers that this is a certain kind of function, a "constructor" function.

We will learn more about constructor functions in the next section, but constructor functions serve the purpose of creating or "constructing" objects that will be created using the new keyword.

In the body of the function, we add two properties on the keyword this, `firstName` and `lastName`, and assign them to the values that are passed to the function.

When we invoke this function, what should we expect to see?

```js
var elie = Person('Elie', 'Schoppik');
elie; // undefined
```

Why did this happen? Remember that functions which do not return any values return `undefined`, so nothing special is happening here. And what about the value of the keyword `this`?

In this case, there's no explicit binding going on, and Person isn't being called as a method on some parent object. So the default binding must take hold, which means that this will be defined as the window object. What we have actually done is defined two properties on the window, `firstName` and `lastName`!

```js
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

window.firstName; // undefined
window.lastName; // undefined

var elie = Person('Elie', 'Schoppik');
elie; // undefined

window.firstName; // 'Elie'
window.lastName; // 'Schoppik'
```

So how do we fix this issue and change the value of the keyword this? We use the `new` keyword. Let's try using it to invoke the `Person` function and examine the value returned.

```js
var elie = new Person('Elie', 'Schoppik') 
elie // {firstName: 'Elie', lastName: 'Schoppik'}
```

So what did the `new` keyword do? It somehow allowed us to return an object with the values specified! In more detail here is what the new keyword does:

- It creates an empty object.
- It sets the value of the keyword `this` to be that empty object created.
- It adds an implicit `return` this in the function. Notice we are not using the word return in our function, and yet our function is still returning an object when we use the new keyword!

The last thing the `new` keyword does is a bit more complex and we will analyze it in much more detail in the next section. The new keyword creates a link between the `prototype` property on the constructor function and the object that was just created. Once again, we will examine this link in much more detail so do not worry about understanding this final point for now.

# Next

When you're ready, move on to [Exercises](./05-exercises.md)