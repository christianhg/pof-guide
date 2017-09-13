# Talking FP

All code examples use [Ramda](http://ramdajs.com/), a functional library for JavaScript.

- [Currying](#currying)
- [Partial Application](#partial-application)
- [Index](#index)

## Currying

Currying is the act of transforming a function that takes multiple arguments into a new function that takes any number of these arguments to produce a new function:

```js
const addThree = R.curry((a, b, c) => a + b + c)

addThree(2, 4, 6)
// => 12

addThree(2, 4)(6)
// => 12

addThree(2)(4, 6)
// => 12

addThree(2)(4)(6)
// => 12

const g = addThree(2)
const h = g(4)
h(6)
// => 12
```

## Partial Application

In functional programming you don't call functions with arguments, you *apply* a function to an argument.

A curried function can be *partially applied* by applying it to *some* of its expected arguments:

```js
const inc = R.add(1)
```

This produces what is known as a *partial* function that can be passed around, e.g. to increment a list of numbers:

```js
[1, 2, 3].map(inc)
// => [2, 3, 4]
```

When the `inc` function is applied to its last argument it is known to be *saturated*.

## Index

- [Currying](#currying)
- [Partial application](#partial-application)
- [Partial function](#partial-application)
- [Saturated function](#partial-application)
