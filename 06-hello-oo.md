# Introduction to Object Oriented Programming in JavaScript

# Objectives

By the end of this chapter, you should be able to:

- Describe OOP without talking about code
- Define encapsulation
- Define abstraction
- Define inheritance

# What is Object Oriented Programming?

Object oriented programming is a method of programming that attempts to model some process or thing in the world as a **class** or **object**. An object in this case is not the same as JavaScript's version of an object. Instead, you can conceptually think of a class or object as something that has data and can perform operations on that data. With object oriented programming, the goal is to encapsulate your code into logical groupings using classes so that you can reason about your code at a higher level. Let's see an example:

# Poker Example

Say we want to model a game of poker in our program. We could write the program using an array to represent the deck of cards, and then other arrays to represent what each player has in their hand. Then we'd have to write a lot of functions to do things like deal, draw cards, see who wins, etc.

When you end up writing large functions involving basic data structures and lots of code in one file, there is usually a better way to organize your code. Instead of trying to do everything at once, we could **separate concerns**.

When thinking about a game of poker, some larger processes and objects stand out that you will want to capture in your code:

- Card
- Deck of cards
- Poker hand
- Poker game
- Discard pile (maybe)
- Player
- Bets

Each one of these components could be a class in your program. Let's pick one of the potential classes and figure out the data that it will hold and the functions that it should be able to perform:

# Deck of cards

- Cards - the deck should have 52 different playing cards
- Shuffle - the deck should be able to shuffle itself
- Deal a card - the deck should be able to remove a card from itself and deal it to a player
- Deal a hand - the deck should be able to deal a hand to a player or set of players

Now that we can conceptualize how a problem can be broken down into classes, let's talk about why programming this way can be useful.

# Encapsulation

Encapsulation is the idea that data and processes on that data are owned by a class. Other functions or classes outside of that class should not be able to directly change the data.

In our deck class, we have 52 cards. The player class should not be able to choose any card he or she wants from the deck or change the order of a deck manually. Instead a player can only be dealt a hand. The contents of the deck is said to be encapsulated into the deck class because the deck owns the array of cards and it will not allow other classes to access it directly.

# Abstraction

Abstraction is the result of a good object oriented design. Rather than thinking about the details of how a class works internally, you can think about it at a higher level. You can see all of the functions that are made available by the class and understand what the class does without having to see all of the code.

Continuing with our example, if you had a deck of cards class and you saw that you could call the `.shuffle()` function or the `.deal()` function, you would have a good understanding of what the class does and what functionality it provides without having to understand how the functions are working internally.

# Inheritance

Inheritance is when a child class inherits functionality from a parent class. For example, a parent class may be an automobile, and it could have a child class for sports car or for truck that has different or more specific characteristics. Both sports cars and trucks share some properties in common but they also have differences that are specific to their own class. So the truck class would inherit functionality from automobile and the sports car class would inherit from automobile as well.

# Polymorphism

Polymorphism may be useful to understand at a high level now, but we will not focus on it when we do OOP in JavaScript. Polymorphism doesn't apply very well to a language like JavaScript.

Polymorphism is the idea that an instance of a child class can be treated as if the child class were the parent class. The child can implement functionality that is specific to its class, but it can be called in the context of the parent. Going back to the automobile example. We could treat both a sports car and a truck as an automobile. An automobile may define an `openTrunk` function. That function could be implemented differently for a sports car versus a truck, but it would still be available to be called by an automobile.

We will not cover these topics in great depth but it's good to understand what they are so that you are not surprised when you read more about OOP, especially in other languages.

# Exercises

- Define _encapsulation_, _abstraction_, and _inheritance_ in your own words.
- Think about how you would design a game of checkers. What data do you want to save? What components are involved in a checkers game that we could turn into a class? What would the relationship be between one class and another?

# Next

When you're ready, move on to [Constructor Functions](./07-constructor.md)

