# path

is a simple file path library for Carp.

## Installation

```clojure
(load "git@github.com:carpentry-org/path@0.1.0")
```

### Usage

The `Path` module mostly operates on `String` arguments. It allows you to
split, join, merge, and normalize paths and extensions in a lot of different
ways. `normalize` resolves `.`/`..` segments and collapses repeated separators
lexically (without touching the filesystem). It also has some functions to work
with the `PATH` environment variable.

It assumes either Windows or POSIX-style separators.

Look at [the documentation](https://carpentry.dev/path) for more information.

<hr/>

Have fun!
