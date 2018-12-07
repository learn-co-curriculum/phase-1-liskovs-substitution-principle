# Liskov's Substitution Principle

# Learning Goals

- Recognize the meaning of strong behavioral subtyping
- Recognize the benefits of upholding Liskov's substitution principle

## Introduction

Much work has been done in the field of Object Oriented programming, and over
the last few decades, engineers have developed design patterns and principles
that are meant to help keep Object Oriented code easier to understand and
maintain. One principle in particular applies to inheritence and extension,
Liskov's substitution principle.

In this lesson, we're going to briefly look at what Liskov's substitution
principle is, how to adhere to it and why.

## Recognize the Meaning of Strong Behavioral Subtyping

Liskov's substitution principle, also known as strong behavioral subtyping,
is the 'L' in [SOLID][solid] a popular set of Object Oriented design principles.

**Liskov's substitution principle:** Objects in a program should be replaceable
with instances of their subtypes without altering the correctness of that
program.

In terms to JavaScript inheritance, an instance of a parent class should be
replaceable with an instance of a child class.

If we follow this principle, the consequence is that properties and methods that
exist on the parent will never be modified in any child. Child classes can
_expand_ upon what they inherited, adding methods or extra properties, but not
change what they inherited.

If you need to modify a child's inherited properties or methods, why are we
inheriting them in the first place?

Below are two examples, one that violates Liskov's principle, and one that
upholds it:

#### Violates Substitution Principle:

```js
class Reptile {
	constructor(name) {
		this.name = name;
	}
	move() {
		return `${this.name} crawls away`;
	}
}

// Lizard inherits `move()` because it crawls
class Lizard extends Reptile {}

//  Snake overrides `move()` because it cannot crawl
class Snake extends Reptile {
	move() {
		return `${this.name} slithers away`;
	}
}
```

The `Snake` class is a subtype of class `Reptile`, but overrides the `move()`
method because the original doesn't apply. If we created an instance of
`Reptile`:

```js
let tricky = new Reptile('Tricky');
```

and an instance of `Snake`:

```js
let basilisk = new Snake('Basilisk');
```

We see that `tricky` cannot be replaced with `basilisk` without changing
behavior:

```js
tricky.move(); // => "Tricky crawls away"
basilisk.move(); // => "Tricky slithers away"
```

#### Upholds Substitution Principle:

So how do we stop violating Liskov's principle? We can either choose to not
inherit from the same parent, _or_ we can create a grandparent class, one
that shares all methods and properties with all children and grandchildren:

```js
class Reptile {
	constructor(name) {
		this.name = name;
	}
}

class LeglessReptile extends Reptile {
	move() {
		return `${this.name} slithers away`;
	}
}

class LeggedReptile extends Reptile {
	move() {
		return `${this.name} crawls away`;
	}
}

class Lizard extends LeggedReptile {}
class Snake extends LeglessReptile {}
```

Now we've got two levels of inheritance. `Reptile` sets up the `constructor`
that all children and grandchildren inherit. Because the `move()` method
needs to behave differently for legged and legless reptiles, it is defined
differently in both `LeglessReptile` and `LeggedReptile`.

With this structure, if an instance of `Lizard` replaced an instance of
`LeggedReptile` _or_ `Reptile`, it will work correctly. The same goes for an
instance of `Snake`.

## Recognize the Benefits of Upholding Liskov's Substitution Principle

Both of the above examples _work_. There is no syntax error if you choose to
ignore Liskov's substitution principle. This is considered a purely _semantic_
design choice.

The benefit of following this principle is that no matter how complicated
inheritance gets, you always assume that whatever a parent class has, a child
class will have too. Looking at a parent class should give us some insight into
the functionality of any children, grandchildren, great grandchildren, etc...

Having chains of inheritance where children might fundamentally change the data
and behaviors they inherit can introduce bugs, but more importantly, can
make your code much more complicated than it needs to be.

A child class may include _more_ properties or use the data differently. If you
do find that a child needs to overwrite something it inherited from its parent,
consider other options - perhaps don't inherit at all.

In general, when dealing with inheritance, the fewer levels of inheritance, the
better. If you've got great grandparent, grandparent, parent and child classes,
it can be difficult to figure out which class contributes what to a child.

It also makes our code more difficult to change. You may not be able to modify
code on a grandparent class without fundamentally changing how a parent or
child class functions. Inheritance can make our code inflexible

Upholding Liskov's substitution principle limits what we can do with inheritance,
discouraging larger chains of inherited classes. As a side effect, our code is
easier to understand and change later on.

## Conclusion

Liskov's substitution principle ensures that all subtypes may replace their
types without altering the behavior of the program. An instance of a parent
class should be replaceable by an instance of any of its child classes an still
work as expected. The result of this pattern is a clearer, consistent
inheritance pattern that leads to more organized, easier to change code.

[solid]: https://en.wikipedia.org/wiki/SOLID
