# Let Bindings

The `:=` symbol is called a let-binding, and it lets you extract the result of a `Transaction` in a concise way. The result of this `let` is a single transaction that only commits if every step succeeded:

```elm
type EmailSecret = EmailSecret Uuid.Uuid

passwordResetInit : Cookies -> Email -> Purpose -> Transaction EmailSecret
passwordResetInit _ email purpose =
  let
    secret  := Uuid.generate EmailSecret
    created := Time.now

    () :=
      insert resetSessions (EmailInit email)
        { secret = secret
        , email = email
        , created = created
        , purpose = purpose
        }
  in
  Transaction.succeed secret

-- Uuid.generate : (Uuid.Uuid -> a) -> Transaction a
-- Time.now : Transaction Time.Posix
-- Table.insert : Table security row -> security -> row -> Transaction ()
```

This makes it easy to use a value like `secret` in multiple places.

Using `:=` in a `let` is completely equivalent to using `Transaction.andThen`. It just looks a bit nicer, especially when there are many steps in a transaction.

## Difference between let bindings and resource bindings

When the `:=` symbol appears in a top-level definition, it has a slightly different purpose. In this context it is called a [resource binding](https://acadia.engineering/documentation/Resource), specifically meant for defining things like tables and sequences. Notice that [`Sequence.uint64`](https://acadia.engineering/documentation/Sequence#uint64) and [`Table.table`](https://acadia.engineering/documentation/Table#table) are producing a [`Resource.Description`](https://acadia.engineering/documentation/Resource#Description), which we then extract with a top-level resource binding.
