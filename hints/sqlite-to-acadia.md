# SQLite to Acadia

So you already have data stored in a SQLite file, `existing.sqlite`, and you would like to start working with Acadia. This can be done by hand by dropping down into normal SQLite operations. The steps would be something like this:

1. Create a fresh Acadia project and define tables that exactly line up with the data you have in your existing SQLite.
2. Publish version 1.0.0 of your Acadia code to an `acadia.sqlite` file. It will have the right Acadia metadata, but no data yet.
3. Use the `sqlite3` command line tool to extract the old data. Run `sqlite3 existing.sqlite` in your terminal, and then run `.dump` to get the data as a sequence of SQL statements.
4. Run `sqlite3 acadia.sqlite` in your terminal, and then add the particular data that you want with normal SQL statements.

The benefits of this process are that it is simple and flexible, but I imagine there will be more automated approaches as the ecosystem of tools for Acadia grows organically.
