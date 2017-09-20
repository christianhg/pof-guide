# Talking FP

> Learn how to talk functional programming and impress your friends.

This is **Talking FP**: a counterpart to [Functional Programming Jargon][1] that aims to be a little more thorough, structured and mathematical.

All code examples are written in JavaScript: a highly popular language that, to a large extend, is well equipped to be written in a functional programming style. More experienced functional programmers will perhaps recognise that JavaScript can [prove painful in the long run][2], but hopefully those will also recognise that it offers a very good starting point.

## Contents

So far the sections are ordered by their level of difficulty, but that ordering might prove unmanageable in the long run. Bear in mind that this is a work in progress.

1. [Domain and Codomain](#domain-and-codomain)
1. [Composition](#composition)
1. [Partial and Total Functions](#partial-and-total-functions)
1. [Currying](#currying)
1. [Partial Application](#partial-application)
1. [Immutability](#immutability)

- [Mathematical Symbols](#mathematical-symbols)
- [Index](#index)
- [References](#references)

---

## Domain and Codomain

Given the sets `X` and `Y`, a function from `X` to `Y` can be denoted:

```math
f: X → Y
```

An example of such a function could be `length` which takes a string and returns a number:

```js
// String → Number
const length = str => str.length

length('foo')
// => 3
```

In this case the set of strings is known as the *domain* of the function and the set of numbers is known as the *codomain* of the function.

## Composition

![Composition](illustrations/composition.svg)

If you got a function `f: X → Y` and a function `g: Y → Z`, you can *compose* them to create a function `g ∘ f: X → Z`. That means you can apply an argument to the composition a `f` and `g` to take it directly from `X` to `Z`.

Say we got `length` from before that takes a String and transforms it into a Number, and we define a new function, `isEven`, which takes a Number and transforms it into a Boolean:

```js
// String → Number
const length = str => str.length

// Number → Boolean
const isEven = n => n % 2 === 0
```

The composition of `length` and `isEven`, `isEven ∘ length`, then transforms a String into a Boolean:

```js
// String → Boolean
const isEvenString = s => isEven(length(s))

isEvenString('foo')
// => false

isEvenString('fizz')
// => true
```

As you can see, function composition is native in JavaScript. It's just about wrapping functions around functions to pass the result of the inner function as the argument to the outer function. However, if you need to compose more than a couple of functions, the [`compose`][5] function from [Ramda][4] might provide some more readability.

Let's define a new function, `getName`, that plucks the `name` property of an Object:

```js
// Object → String
const getName = o => o.name
```

We can now create the following composition, `isEven ∘ length ∘ getName`, to find out if a person's name is even:

```js
// Object → Boolean
const hasEvenName = R.compose(isEven, length, getName)

hasEvenName({
  name: 'Aron',
  age: 25
})
// => true

hasEvenName({
  name: 'Bob',
  age: 42
})
// => false
```

The composition might as well have been written:

```js
const hasEvenName = o => isEven(length(getName(o)))
```

But using Ramda's `compose` we managed to create the same function in a more concise way.

## Partial and Total Functions

In programming it is highly likely that you'll produce functions that don't return a value for all inputs. Consider this version of `sqrt` which expects all but negative numbers:

```js
// Number → Number
const sqrt = n => n >= 0 ? Math.sqrt(n) : undefined

sqrt(4)
// => 2

sqrt(-4)
// => undefined
```

In the example of `sqrt`, not all members of the domain map to the codomain which makes it a *partial function*. The sister function, `sqr` doesn't have this limitation and is thus known as a *total function*:

```js
// Number → Number
const sqr = n => n * n

sqr(-2)
// => 4

sqr(2)
// => 4
```

## Currying

Currying is the act of transforming a function that takes multiple arguments into a new function that takes these arguments one at a time or all at once. Consider `addThree` which takes three numbers and produces a new number:

```js
// Number × Number × Number → Number
const addThree = (a, b, c) => a + b + c
```

By using the [curry][3] function from the [Ramda][4] library, we can turn it into a *curried function*:

```js
// Number → Number → Number → Number
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

## Immutability

![Immutability](illustrations/immutability.svg)

What's cool about version control systems like [Git][6]? They're *immutable*! Every state change made to a project through Git preserves the previous state in history. Why is this cool? For starters it allows a distributed team to work on the same project without messing up each others work. Unless [rebases][7] commonly occur in your team, the immutable Git history means that you can checkout a commit and have absolute certainty that nobody can change that state of the project from under your feet.

The exact same argument can be made from the perspective of code itself. In functional programming values are (preferably) immutable. This means that the name associated with the value, the variable, can't be associated with another value, and that the value itself can't change. This provides certainty and transparency in the same sense that the Git workflow does. As soon as a part of the program obtains a value to work on, it can do that with a guarantee that the value doesn't suddenly change.

Some languages, like [Haskell][8], enforce immutability, while others do no such thing. In the case of JavaScript, immutability can indeed be a very hard thing to strive for.

---

## Mathematical Symbols

|Symbol|Name|Explanation|
|---|---|---|
|→|Function arrow|Denotes a function that maps between one set to another.|
|×|Multiplication|Denotes the cross product of two inputs.|
|∘|Function composition|Denotes the composition of two functions.|

## Index

- [Curried function](#currying)
- [Currying](#currying)
- [Function domain](#domain-and-codomain)
- [Function codomain](#domain-and-codomain)
- [Function composition](#composition)
- [Immutability](#immutability)
- [Partial application](#partial-application)
- [Partial function](#partial-and-total-functions)
- [Ramda](#currying)
- [Saturated function](#partial-application)
- [Total functions](#partial-and-total-functions)

## References

- [LambdaCast](https://soundcloud.com/lambda-cast)
- [List of mathematical symbols](https://en.wikipedia.org/wiki/List_of_mathematical_symbols)
- Spivak, D.I. (2014) *Category Theory for the Sciences*

[1]: https://github.com/hemanth/functional-programming-jargon "Functional Programming Jargon"
[2]: https://hackernoon.com/functional-programming-in-javascript-is-an-antipattern-58526819f21e "Functional programming in Javascript is an antipattern"
[3]: http://ramdajs.com/docs/#curry "Curry in Ramda"
[4]: http://ramdajs.com/ "Ramda"
[5]: http://ramdajs.com/docs/#compose "Compose in Ramda"
[6]: https://git-scm.com/ "Git"
[7]: https://git-scm.com/docs/git-rebase "Git rebase"
[8]: https://www.haskell.org/ "Haskell"
