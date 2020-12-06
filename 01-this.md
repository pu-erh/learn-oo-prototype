# Introduction to the keyword 'this'.

# Objectives:

By the end of this chapter, you should be able to:

- Explain what the keyword this is and what a call-site is
- Examine the first way of determining the value of the keyword this: default binding

# What is the meaning of `this`?

The keyword `this` is one of the more unique features of JavaScript. It often presents a stumbling block to people learning JavaScript, even if they already know some other language. However, the rules governing `this` are relatively straightforward once you've familiarized yourself with them.

At its core, `this` is just a keyword in JavaScript that refers to some object. For instance, if you hop into the Chrome console and type `this`, you'll see that it refers to the window. However, the value of the keyword `this` depends on where in your code you use it. Let's take a look at the different ways the value for the keyword `this` gets set.

# The four ways to figure out the keyword this

The terms `default binding, implicit binding, explicit binding`, and `new binding` come from the incredible book You Don't Know JS by Kyle Simpson. You can read more about these terms in his book on prototypes and the keyword this [here](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md).

# Default Binding

You can think of this as the last possible option when all other rules do not apply. The default binding exists when the keyword this is in the global context. As we've already seen, in the case of the browser the keyword this has a default binding to the `window`.

```js
var thing = this;
thing; // window

function outer() {
    return this;
}

outer(); // window
```

For the other three bindings, the value of the keyword this is set to some value other than the default when it is used inside of a function. But the details of how the value gets set depend on the call-site of the function. The call-site is simply the way and location in which the function is called.

# Strict Mode

To prevent ourselves from creating variables in the global scope, we can add the text "use strict" to enable strict mode. This was created in ES5 to prevent developers from making mistakes and instead of silent errors, strict mode will throw errors. You can read more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

If `strict mode` is enabled, the default binding of `this` is `undefined`.

```js
"use strict"

function outer() {
    return this;
}

outer(); // undefined

function info(){
    this.data = "something";
}

info() // Uncaught TypeError: Cannot set property 'data' of undefined
// this is happening since `this` is being set to undefined
// and we can not access properties on undefined!
```

# Next

When you're ready, move on to [Implicit Binding](02-implicit.md)

