
# Hints for Bad Recursion

Recursive values and functions are not allowed in database modules. The machine running the database should focus on loading data. If further processing is needed, that should happen separately in a server module. This sets you up for a more “scalable” system design:

```
      ┌─────────────┐
      │load balancer│
      └─────────────┘
         │   │    │
   ┌─────┘   │    └─────────┐
   │         │              │
┌──────┐  ┌──────┐       ┌──────┐
│server│  │server│  ...  │server│
└──────┘  └──────┘       └──────┘
   │         │              │
   └───────┐ │    ┌─────────┘
           │ │    │
          ┌────────┐
          │database│
          └────────┘
```

If the processing work becomes a bottleneck, you can add more servers. That work just needs a CPU. It is harder to “shard” databases in a sensible way, so you get farther if you minimize the work that happens on the database.

The lack of recursion is inspired by [Datalog](https://en.wikipedia.org/wiki/Datalog) and leads to some interesting guarantees:

- All database queries terminate! There is no way to express a query that keeps running forever.
- It is impossble to write 1+N queries! A “1+N query” is when you do a single query, and then loop over the resulting rows to do N more queries. This can happen in pl/pgSQL and Object-Relational Mappings (ORMs) that have `for` loops. These cases can always be better expressed as a `JOIN`, and in Acadia, that is the only way to express it!

