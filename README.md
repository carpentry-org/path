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
lexically (without touching the filesystem), and `relative` expresses one path
relative to a base the same way. It also has some functions to work with the
`PATH` environment variable.

It assumes either Windows or POSIX-style separators.

### Glob matching

`matches?` checks a path against a glob pattern, and `matching` keeps the paths
in an array that match one, in order. Both are lexical as well and never touch
the filesystem, and both take the path first, like everything else here — note
that this is the opposite of `Pattern.matches?` in core.

```clojure
(Path.matches? "src/main.carp" "src/*.carp")         ; => true
(Path.matching &[@"a.carp" @"b.c"] "*.carp")         ; => [@"a.carp"]
(Path.matching &[@"a.carp" @"t/b.carp"] "**/*.carp") ; => [@"a.carp" @"t/b.carp"]
```

| Form     | Meaning                                              |
|----------|------------------------------------------------------|
| `?`      | exactly one character, never a separator             |
| `*`      | zero or more characters, never a separator           |
| `**`     | zero or more whole segments, as an entire segment    |
| `[abc]`  | one of the characters in the class                   |
| `[a-z]`  | one character from the range                         |
| `[!abc]` | one character not in the class (`[^abc]` also works) |
| `\*`     | a literal `*`, on POSIX                              |

`**` is only a segment wildcard when it is the whole segment, so `**/*.carp`
matches both `a.carp` and `x/y/a.carp`, while `a**b` is just `a*b`. On POSIX a
`\` escapes the next pattern character; on Windows `\` is a separator, so
escaping is disabled there. A trailing `\` and an unterminated `[` are matched
as literal characters, which is why `matches?` is a plain `Bool` and not a
`Result`. Separators are structural and cannot be escaped away: `a\/b` splits
into segments just like `a/b`, and a class holding one, such as `[a/]`, matches
its other members but never the separator.

Leading dots are not special: `*` matches `.hidden`. Matching happens on the
path exactly as given, with no normalization, so run it through `normalize`
first if `.`, `..` or repeated separators should not get in the way.

Look at [the documentation](https://carpentry.dev/path) for more information.

<hr/>

Have fun!
