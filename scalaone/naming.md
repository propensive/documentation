## Names in Scala One projects

### Precision and Concision

Various factors influence the choice of names for terms and types. Most
importantly, names should be chosen that clearly indicate what the entity is
(or represents). The name should be sufficiently precise, but not more precise
than would be correct.

This almost goes without saying, but other considerations also apply.

Abbreviations should be avoided, except when the abbreviation is well
established (even if only established in a technical domain) and the
unabbreviated name is long enough to be warrant shortening. So `concat` is an
acceptable abbreviation of "concatenation" or "concatenate" because it's
well established, whereas `pp` would not be a good name for "pages" because
"pages" is short already (even though `pp` has been established for centuries).
The abbreviation `mgmt` would not be justified as a short form of "management"
because it's not so well established. 

Superfluous words should be avoided in names. There is no need to write every
"getter" in the form `getX`, or to include the type in a term's name.

One exception to this rule is exceptions, which are exceptional enough that
each has a name ending in `Error`, for example, `StreamCutError`. (The idea of
using `Exception` instead of `Error`, following Java's convention, was rejected
because exception names are already long, and saving four characters hundreds
of times over was deemed worthwhile.)

### Consistency

Where possible, especially if subjectivity has set an earlier precedent in a
naming choice, names for similar things should be formed in similar ways.

For example, since `Doc` (itself an established abbreviation for "document")
would mean different things in different contexts, and `JsonDoc` and `XmlDoc`
existed already, subsequent types representing documents of HTML and
[CoDL](https://github.com/propensive/codl/) were named `HtmlDoc` and `CodlDoc`.
However slight, it's an advantage to the programmer to be able to predict a
type's name before they have seen it.

Likewise, there is an established convention for naming contextual typeclass
instances, of the form `Typeclass[TypeParam]`. Such typeclasses will typically
reside in the `Typeclass` companion object or the `TypeParam` companion,
depending on which is under the programmer's control in the definition project.
The name of the `given` definition should be chosen as whichever part of the
type that is not already conferred by the enclosing object. So, for example, an
instance of `Streamable[Json]` in the `Json` companion object should be called
`streamable`, whereas an instance of `Streamable[Json]` in the `Streamable`
companion should be called `json`.

Here are some other examples which apply, by precedent, in Scala One projects:
- singleton objects for macro definitions should be named after the package,
  followed by `Macros`, for example `probably.ProbablyMacros`,
  `adversaria.AdversariaMacros`, `honeycomb.HoneycombMacros`
- unique parameters of a particular type should have the same name as that
  type, if the name makes sense; an abbreviated form should not be used, nor
  should a parameter use its type name if other parameters exist of the same type
- converters to generic types like JSON or XML that are defined as extension
  methods should take the name of the resultant type, e.g. `javanais.json`,
  `xylophone.xml`, `cellulose.codl`, `punctuation.markdown`
- exception types should all be suffixed with `Error`
- generic typeclass instances in
  [Anticipation](https://github.com/propensive/anticipation/) should be
  prefixed with `Generic`
- readers and writer typeclasses (sometimes called encoders and decoders)
  should be named `Reader` and `Writer` prefixed with the type they decode and
  encode for, e.g. `javanais.JsonReader`, `javanais.JsonWriter`,
  `xylophone.XmlReader`

These conventions are not exhaustive, nor are they universally applied, but any
deviation from them (without good reason) should be considered a _code quality_
issue.

### Acronyms

Acronyms, such as GADT or URL, which are normally written in all-caps, should
be used in names like any other word, rendered in lowercase, or with just an
initial capital letter. So as a method name, "URL" would be written `url`; as a
type, `Url`; or as part of a method name, for example, `toUrl`. The "R" and "L"
should never be upper-case.

As with other abbreviations, acronyms should only be used if their meaning is
already established and widely understood.

### Universal Uniqueness

Across all Scala One libraries, no two top-level types or objects should have
the same name. This ensures that a programmer can confidently import everything
from any number of Scala One projects without fear of introducing a naming
ambiguity, and can refer to any type or object without reference to its
enclosing package. (The rule does not apply to nested entities, though.)

This means that new names should be checked for homographms across all Scala
One projects, unless their uniqueness is obvious.

Sometimes, in the event of a conflict, a distinct synonym or near-synonym can
suggest a good alternative, but it should not be used in preference to a name
that is already used, by precedent, to refer to the same thing.

If, for example, the name `Tree` were used universally to refer to a tree, then
we could not justify using `Hierarchy` as the name of a type parameter, a
subtype of `Tree`, just because it also refers to a tree-like entity and is
conveniently distinct. The only justification for using both `Tree` and
`Hierarchy` would be if there were a material difference in the entity they
refer to. A better choice for the parameter's name would be `TreeType`.

