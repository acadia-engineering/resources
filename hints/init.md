
# Creating an Acadia project

The main goal of `acadia init` is to get you to this page!

It just creates an `acadia.json` file and a `src/` directory for your code.


## What is `acadia.json`?

This file describes your project. By default, all your code should live in the `src/` directory, but you can add additional source directories if you want.


## What goes in `src/`?

This is where all of your Acadia files live. It is best to start with a file called something like `src/Backend.db`. As you work through [the examples](https://github.com/acadia-engineering/examples), you can put the code examples in that `src/Backend.db` file.


## How do I run it?

There are three steps to getting a database running:

```bash
acadia make --gen-elm=gen/
# Generate endpoints and types in Elm files in the gen/ directory.
# You can then `import` those endpoints and types in Elm directly.

elm make src/Main.elm --output=index.html
# Generate some HTML that uses your database endpoints.

acadia serve --html=index.html
# Serve your client and database from http://localhost:9000.
# This serves index.html for all GET requests on all paths.
```

In more advanced projects, you probably want to serve the HTML and JS in a more sophisticated way. The `--html` flag exists as a convenience, to make it easy to start experimenting.


## How do I structure my directories?

Many people get anxious about their project structure. “If I get it wrong, I am doomed!” This anxiety makes sense in languages where refactoring is risky, but Acadia is not one of those languages!

We recommend that newcomers stay in one file until you get into the 600 to 1000 range. Push out of your comfort zone. Having the experience of being fine in large files will help you understand the boundaries in Acadia, rather than just defaulting to the boundaries you learned in another language.

The talk [The Life of a File](https://youtu.be/XpDsk374LDE) gets into this in the context of Elm, so that may be useful even if database modules end up wanting to be built around tables rather than particular custom types.


## How do I start fancier projects?

I wanted `acadia init` to generate as little code as possible. It is mainly meant to get you to this page! If you would like a more elaborate starting point, I recommend starting projects with commands like this:

```bash
# Note: This particular repo has not been created yet! In the spirit of posting more frequently,
# I am trying to break my work into independently publishable chunks. So I will set this repo up
# sometime in the next few weeks.
git clone https://github.com/evancz/acadia-todomvc.git
```

The idea is that Acadia projects should be so simple that nobody needs a tool to generate a bunch of stuff. This also captures the fact that project structure _should_ evolve organically as your application develops, never ending up exactly the same as other projects.

But if you have something particular you want, I recommend creating your own starter recipe and using `git clone` when you start new projects. That way (1) you can get exactly what you want and (2) we do not end up with a complex `acadia init` that ends up being confusing for beginners!
