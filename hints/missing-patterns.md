
# Hints for Missing Patterns

Acadia checks to make sure that all possible inputs to a function or `case` are handled. This gives us the guarantee that no Acadia code is ever going to crash because data had an unexpected shape.

There are a couple techniques for making this work for you in every scenario.


## The danger of wildcard patterns

A common scenario is that you want to add a tag to a custom type that is used in a bunch of places. For example, maybe you are working different variations of users in a chat room:

```elm
type User
  = Regular String Int
  | Anonymous

toName : User -> String
toName user =
  case user of
    Regular name _ ->
      name

    _ ->
      "anonymous"
```

Notice the wildcard pattern in `toName`. This will hurt us! Say we add a `Visitor String` variant to `User` at some point. Now we have a bug that visitor names are reported as `"anonymous"`, and the compiler cannot help us!

So instead, it is better to explicitly list all possible variants, like this:

```elm
type User
  = Regular String Int
  | Visitor String
  | Anonymous

toName : User -> String
toName user =
  case user of
    Regular name _ ->
      name

    Anonymous ->
      "anonymous"
```

Now the compiler will say "hey, what should `toName` do when it sees a `Visitor`?" This is a tiny bit of extra work, but it is very worth it!



By demanding the first element of the list as an argument, it becomes impossible to call this function if you have an empty list!

This “unroll the list” trick is quite useful. I recommend using it directly, not through some external library. It is nothing special. Just a useful idea!
