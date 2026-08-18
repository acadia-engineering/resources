# Comparing Records

The built-in comparison operators work on a fixed set of types, like `Int64` and `String`. That covers a lot of cases, but what happens when you want to compare records?


## Sorting Records

What is the proper sort order for records? Should it be by source order? Alphabetical order? Each option has quite serious problems:

- **Source Order** &mdash; Acadia lets you write record fields in whatever order you want, so `{ x = 3, y = 4 }` has the the same type as `{ y = 4, x = 3 }`. In other words, Acadia does not have a concept of “source order” of record fields in the first place.
- **Alphabetical Order** &mdash; First, this would be fairly surprising to many people who might expect “the order I wrote in my type alias” to be the default ordering. Second, the order of unicode code points does not match the concept of “alphabetical” in many languages. Take the Icelandic alphabet `aábdðeéfghiíjklmnoóprstuúvxyýzþæö` as an example. The letter `á` appears between `a` and `b`. The unicode order of Bengali letters is not even close to what is expected by the 242 million native speakers (as of 2025). And the concept of “alphabetical” simply has no practical meaning for the Chinese characters used by over 1.4 billion people (as of 2024).

Rather than picking an order that will inevitably surprise some fraction of programmers, we recommend making the order explicit with tuples.


## Using Tuples

Tuples are sorted from left to right, so `(x,y,z)` would first compare `x`, then `y`, and finally `z`.

So if you have a bunch of records you want to sort, you can map the fields you care about into a tuple. You can also use nested tuples like `("Bob", 181, (1965, "Texas"))` if you need to surpass the tuple size limit of three entries.

In general, Acadia does not need to allocate intermediate values. The concept of “functions” and “tuples” do not exist in SQL, so you are going to get a normal `ORDER BY` clause regardless of how it is expressed in Acadia code. So you do not need to worry that converting a record to a tuple might impose some additional overhead.
