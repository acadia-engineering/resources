# Comparing Custom Types

The built-in comparison operators work on a fixed set of types, like `Int64` and `String`. That covers a lot of cases, but what happens when you want to compare custom types?

This page aims to catalog these scenarios and offer alternative paths that can get you unstuck.


## Simple Cases

It is common to try to get some extra type safety by creating really simple custom types:

```elm
type ID = ID Int
type Age = Age Int

type Comment = Comment String
type Description = Description String
```

This kind of type always takes on the `equatable`, `hashable`, `storable`, and `comparable` behavior of the wrapped type. So they can be used as primary keys, hash keys, etc.

By wrapping the primitive values like this, the type system can now help you make sure that you never mix up a `ID` and an `Age`. Those are different types! This trick is extra nice because it has no runtime cost.


## Hashable Cases

Custom types are `hashable` if all of the associated data is also `hashable`. So if you want a more elaborate custom type to be used as a dictionary key, you can use its hash as the key.
