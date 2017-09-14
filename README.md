# Talking FP

> Learn how to talk functional programming and impress your friends.

All code examples are written in JavaScript and have an implicit dependency on [Ramda](http://ramdajs.com/).

- [Domain and Codomain](#domain-and-codomain)
- [Partial and Total Functions](#partial-and-total-functions)
- [Currying](#currying)
- [Partial Application](#partial-application)
- [Index](#index)

---

## Domain and Codomain

Given the sets `X` and `Y`, a function from `X` to `Y` can be denoted:

```math
f: X ⟶ Y
```

An example of such a function could be `length` which takes a `String` and returns a `Number`:

```js
// String ⟶ Number
const length = str => str.length

length('foo')
// => 3
```

In this case the set of strings is known as the *domain* of the function and the set of numbers is known as the *codomain* of the function.

## Partial and Total Functions

In programming it is highly likely that you'll produce functions that don't return a value for all inputs. Consider this version of `sqrt` which expects all but negative numbers:

```js
// Number ⟶ Number
const sqrt = n => n >= 0 ? Math.sqrt(n) : undefined

sqrt(4)
// => 2

sqrt(-4)
// => undefined
```

In the example of `sqrt`, not all members of the domain map to the codomain which makes it a *partial function*. The sister function, `sqr` doesn't have this limitation and is thus known as a *total function*:

```js
// Number ⟶ Number
const sqr = n => n * n

sqr(-2)
// => 4

sqr(2)
// => 4
```

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

In functional programming you don't call functions with arguments, you *apply* them. `f(x)` is read "apply `f` with `x`".

A curried function can be *partially applied* by applying it to *some* of its expected arguments:

```js
const inc = R.add(1)
```

This produces a new function that can be passed around, e.g. to increment a list of numbers:

```js
[1, 2, 3].map(inc)
// => [2, 3, 4]
```

When the `inc` function is applied to its last argument it is known to be *saturated*.

---

## Index

- [Currying](#currying)
- [Function domain](#domain-and-codomain)
- [Function codomain](#domain-and-codomain)
- [Partial application](#partial-application)
- [Partial function](#partial-and-total-functions)
- [Saturated function](#partial-application)
- [Total functions](#partial-and-total-functions)
