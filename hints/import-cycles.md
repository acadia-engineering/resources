
# Import Cycles

What is an import cycle? In practice you may see it if you create two modules with interrelated `User` and `Comment` types like this:

```elm
database module Comment exposing (..)

import User

type alias Comment =
  { comment : String
  , author : User.User
  }
```

```elm
database module User exposing (..)

import Comment

type alias User =
  { name : String
  , comments : List Comment.Comment
  }
```

Notice that to compile `Comment` we need to `import User`. And notice that to compile `User` we need to `import Comment`. We need both to compile either!

Now this is *possible* if the compiler figures out any module cycles and puts them all in one big file to compile them together. That seems fine in our small example, but imagine we have a cycle of 20 modules. If you change *one* of them, you must now recompile *all* of them. In a large code base, this causes extremely long compile times. It is also very hard to disentangle them in practice, so you just end up with slow builds. That is your life now.

The thing is that you can always write the code *without* cycles by shuffling declarations around, and the resulting code is often much clearer.

A common technique for resolving these cycles is to collapse the interrelated types and values into a single module. From there you can get the code working and assess if there are further implicationns for your module structure.
