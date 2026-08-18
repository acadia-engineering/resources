
# Hints for Imports

When getting started with Acadia, it is pretty common to have questions about how the `import` declarations work exactly.


<br>

## `import`

An Acadia file is called a **module**. To access code in other files, you need to `import` it!

So say you want to use the [`Table.access`](https://acadia.engineering/documentation/Table#access) function. The simplest way is to import it like this:

```elm
import Rows
import Table

getRows table =
  Table.access () table
    |> Rows.selectAll
```

After saying `import Table` we can refer to anything inside that module as long as it is *qualified*. This works for:

  - **Values** &mdash; we can refer to `Table.access`, `Table.update`, etc.
  - **Types** &mdash; We can refer to [`Constraint`](https://acadia.engineering/documentation/Table#Constraint) as `Table.Constraint`.

So if we add a type annotation to `getRows` it would look like this:

```elm
import Rows
import Table
import Transaction

getRows : Table.Table () row -> Transaction.Transaction (Rows.Rows row)
getRows table =
  Table.access () table
    |> Rows.selectAll
```

We are referring to the [`Table`](https://acadia.engineering/documentation/Table#Table) type, using its *qualified* name `Table.Table`. This can feel weird at first, but it starts feeling natural quite quickly!


<br>

## `as`

It is best practice to always use *qualified* names, but sometimes module names are so long that it becomes unwieldy. This might come up for the `Transaction` module. We can use the `as` keyword to help with this:

```elm
import Transaction as T

okay : T.Transaction ()
okay =
  T.succeed ()
```

Saying `import Transaction as T` lets us refer to any value or type in `Transaction` as long as it is qualified with an `T`. So now we can refer to [`succeed`](https://acadia.engineering/documentation/Transaction#succeed) as `T.succeed`.


<br>

## `exposing`

In quick drafts, maybe you want to use *unqualified* names. You can do that with the `exposing` keyword like this:

```elm
import Rows exposing (..)
import Table exposing (Table, access)
import Transaction exposing (Transaction)

getRows : Table () row -> Transaction (Rows row)
getRows table =
  access () table
    |> selectAll
```

Saying `import Rows exposing (..)` means I can refer to any value or type from the `Rows` module without qualification. Notice that I use the `Rows` type and the `selectAll` function without qualification in the example above.

> **Note:** It seems neat to expose types and values directly, but it can get out of hand. Say you `import` ten modules `exposing` all of their content. It quickly becomes difficult to figure out what is going on in your code. “Wait, where is this function from?” And then trying to sort through all the imports to find it. Point is, use `exposing (..)` sparingly!

Saying `import Transaction exposing (Transaction)` is a bit more reasonable. It means I can refer to the `Transaction` type without qualification, but that is it. You are still importing the `Transaction` module like normal though, so you would say `Transaction.map` or `Transaction.fail` to refer to other values and types from that module.


<br>

## `as` and `exposing`

There is one last way to import a module. You can combine `as` and `exposing` to try to get a nice balance of qualified names:

```elm
import Rows exposing (..)
import Table as Tbl exposing (Table)
import Transaction exposing (Transaction)

getRows : Table () row -> Transaction (Rows row)
getRows table =
  Tbl.access () table
    |> selectAll
```

Notice that I refer to `Tbl.access` which is qualified and `Table` which is unqualified.


<br>

## Default Imports

We just learned all the variations of the `import` syntax in Acadia. You will use some version of that syntax to `import` any module you ever write.

It would be the best policy to make it so every module in the whole ecosystem works this way. We thought so in the past at least, but there are some modules that are so commonly used that the Acadia compiler automatically adds the imports to every file. These default imports include:

```elm
import Basics exposing (..)
import Pair
import Maybe exposing (Maybe(..))
import Result exposing (Result(..))
import String exposing (String)
import Char exposing (Char)
import Table exposing (Table)
import Transaction exposing (Transaction)
```

You can think of these imports being at the top of any module you write.

One could argue that `Maybe` is so fundamental to how we handle errors in Acadia code that it is *basically* part of the language. One could also argue that it is extraordinarily annoying to have to import `Maybe` once you get past your first couple weeks with Acadia. Either way, we know that default imports are not ideal in some sense, so we have tried to keep the default imports as minimal as possible.

> **Note:** Acadia performs dead code elimination, so if you do not use something from a module, it is not included in the generated code. So if you `import` a module with hundreds of functions, you do not need to worry about the size of your assets. You will only get what you use!
