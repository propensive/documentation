Amok files, with the extension `.amok`, contain documentation about some
external topic, usually some code or data in a particular format. The file is
written in a dialect of [CoDL](https://codl.dev/), and its structure will
mostly mirror the structure of the external topic. The keywords used to
describe the structure and its documentation in CoDL will depend on the format.
Most of the documentation will be in embedded
[Markdown](https://en.wikipedia.org/wiki/Markdown).

Amok was designed primarily to document code in Scala, a complex programming
language, and Java, as well as related documentation, such as tutorials, which
has its own (usually flatter) structure. But the hierarchical nature of
entities in Scala and Java lends itself well to other languages, and Amok will
support more languages in the future.

Using CoDL has several advantages. The syntax has a clean aesthetic, using only
whitespace and, (only in certain places) the `#` character, for markup. This
means that almost any other language can be embedded within it without special
characters needing escaping.

The lack of symbolic markup helps with Git diffs too, as there are never
_incidental but insignificant_ changes to lines, like adding additional
commas.

CoDL schemas can also provide a rich, interacive development experience in an
IDE. Although this is not yet ready, a CoDL plugin for Visual Studio Code is in
development and will make it easy to edit CoDL documents and highlight errors
in real-time using a schema.

Crucially, CoDL is optimized for _collaborative_ editing, where both humans and
software may make modifications to a file. CoDL ensures that edits made by
software will retain artifacts like custom formatting and comments, changing
only what's necessary. This is particularly useful when code changes, and the
documentation needs to chance accordingly, automatically updated by Amok.

Here is some code in Scala,
```scala
package mathematics

object Arithmetic:
  def add(a: Int, b: Int): Int = a + b
  def subtract(a: Int, b: Int): Int = a - b
```
and an Amok document describing it:
```amok
pragma amok
scala
  term mathematics   a package containing mathematical utilities
    term Arithmetic  collection of arithmetic operations on integers
      term add       adds two numbers together
        term a       the left operand
        term b       the right operand
      term subtract  subtracts one number from another
        term a       the left operand
        term b       the right operand
```

This simple structure should be intuitive: it provides a brief explanation of
every named entity in the source file, in a hierarchy which mirrors the source
hierarchy, down to the parameter names. For Scala, Amok does not need to know
that the entities are a mixture of `def`s, `package`s, `object`s and
parameters; they all exist within the _term_ namespace, and the keyword `term`
is used to introduce them.

We have given each entity a brief description on each line. These have all been
vertically aligned for visual clarity, but the most important thing is that
there are two spaces between the identifier and the description, to ensure that
the description (which never contains two or more consecutive spaces) is read
as a single string.

There is also no need to provide additional details about each term that can be
inferred from the source code will be captured when the documentation is
assembled.

In Scala, two namespaces exist side-by-side; we also need to document _types_.
If our code sample had included a type, for example,
```scala
package mathematics

case class Decimal(significant: Long, fractional: Long)
```
then our documentation would look like,
```amok
pragma amok
scala
  term mathematics      a package containing mathematical utilities
    type Decimal        a representation of a decimal value
      term significand  the digits to the left of the decimal point
      term fractional   the digits to the right of the decimal point
```
where `term` entities may contain `type` entities, and `type` entities may
contain `term` entities. This is consistent with Scala's model of types and
terms, as two independent but coexisting namespaces. As with terms, Amok treats
different types such as `class`es, `trait`s or `opaque type`s as just `type`s.

Note that if a type has a corresponding companion object, which will have the
same name, then a `type` and `term` for each may appear alongside each other in
the Amok document.

The examples so have shown how a short phrase can be associated with a type or
term, which is useful when a succinct description is enough. But good
documentation will often include more elaborate text, potentially with examples
and links. Amok supports this, with Markdown used as the authoring language.
Here is an example:

```amok
pragma amok
scala
  term mathematics  a package containing mathematical utilities
      The `mathematics` package contains a small, but growing, collection of
      methods and utilities for working with numbers, including,

      - [algebra](https://en.wikipedia.org/wiki/Algebra/),
      - [group theory](https://en.wikipedia.org/wiki/Group_Theory/), and,
      - [equations](https://en.wikipedia.org/wiki/Equation/)

    type Decimal  a representation of a decimal value
        A `Decimal` value represents a decimal number whose significand and
        fractional digits are both represented by 64-bit `Long` values.
```

This shows a short Markdown extract describing (in more detail) the
`mathematics` package and the `Decimal` type within it, while the short
descriptions remain as a distinct piece of documentation.

The Markdown content appears over several lines following each `term` or `type`
declaration, indented an additional four spaces from the left. This makes the
body content the third parameter to either a `term` or a `type` declaration,
albeit a long value which spans several lines. The `Decimal` example above
could be written more explicitly in CoDL by specifying each parameter as a
named child, as in this fragment:
```amok
term
  id Decimal
  info
      a representation of a decimal value
  body
      A `Decimal` value represents a decimal number whose significand and
      fractional digits are both represented by 64-bit `Long` values.
```

The same parameter names, `id`, `info` and `body` are used for `type`s, too.

The embedded Markdown may include references to types and terms, written in
backticks, for example `` `Date` ``. This may be intended to refer to the type
`aviation.Date`, and should be linked to `aviation.Date` whenever it is
presented, but it would be tedious to have to write `aviation.Date` instead of
`Date` everywhere.

Amok provides _imports_ to make this easier. An import brings another namespace
into scope so that it can be referenced without a prefix, and appears as an
`import` declaration alongside `term` and `type` declarations in an Amok
document, for example,

```amok
type Event  a representation of something that happens
  import aviation.*
  term date      the `Date` the event happens
  term time      the `Time` the event happens
  term location  the `geolocation.Coordinates` of where the event happens
```



