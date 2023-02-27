Good, clear and easily-understood code samples help make great documentation.
Syntax highlighting can make make the code easier to read, and more attractive
to look at. Compiling code samples makes them more trustworthy.

But code samples often need to communicate more than just the code. They may
have certain keywords or identifiers which should grab the reader's focus, or
parameters which need to be replaced with the reader's own values. They may
contain errors, which should be highlighted in red. Terms or types may have
longer, fully-qualified names which should be visible by hovering over them.
And some code may be necessary to compile code, but 

These various forms of markup are all supported by Amok, and can be applied
without littering the code with inline markup. For most languages, it's
difficult to unambiguously add markup without conflicting with existing syntax
or distracting from the code when it needs to be edited.

Instead, code fragments can be marked up by specifying the exact text to be
italicized, emboldened, reddened or underlined. This is specified in a prelude
to the code sample, written in CoDL, for example:
```amok
scala
  mark red focus
##
def color(red: Int, green: Int, blue: Int): Double =
  (red << 16) + (green << 8) + blue
```

This will highlight the first occurrence of the substring `red` in the code,
which begins after the delimiter, `##`. Where necessary, additional conditions
can be added to more precisely specify exactly which substring or substrings to
markup.

```amok
scala
  mark  red       focus
  mark  : Double  omissible
##
def color(red: Int, green: Int, blue: Int): Double =
  (red << 16) + (green << 8) + blue
```
