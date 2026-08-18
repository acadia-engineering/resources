
# Hints for Type Annotation Problems

At the root of this kind of issue is always the fact that a type annotation in your code does not match the corresponding definition. Now that may mean that the type annotation is “wrong” or it may mean that the definition is “wrong”. The compiler cannot figure out your intent, only that there is some mismatch.

This document is going to outline the various things that can go wrong and show some examples.


## Annotation vs. Definition

The most common issue is with user-defined type variables that are too general. So let's say you have defined a function like this:

```elm
addPair : (a, a) -> a
addPair (x, y) =
  x + y
```

The issue is that the type annotation is saying "I will accept a tuple containing literally *anything*" but the definition is using `(+)` which requires things to be numbers. So the compiler is going to infer that the true type of the definition is this:

```elm
addPair : (number, number) -> number
```

So you will probably see an error saying "I cannot match `a` with `number`" which is essentially saying, you are trying to provide a type annotation that is **too general**. You are saying `addPair` accepts anything, but in fact, it can only handle numbers.

In cases like this, you want to go with whatever the compiler inferred. It is good at figuring this kind of stuff out ;)


## Annotation vs. Itself

It is also possible to have a type annotation that clashes with itself. This is probably more rare, but someone will run into it eventually. Let's use another version of `addPair` with problems:

```elm
addPair : (Int32, Int32) -> number
addPair (x, y) =
  x + y
```

In this case the annotation says we should get a `number` out, but because we were specific about the inputs being `Int32`, the output should also be an `Int32`.

