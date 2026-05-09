# Whitespace in Mustache

*(This document is non-normative and summarizes the whitespace rules presented in the specs for implementation authors.)*

## Terms

<dl>
<dt>element</dt>
<dd>A section, inverted section, parent, or block tag plus its content up to and including its associated end tag. For example, <code>{{$foo}}bar{{/foo}}</code> is a single block element.</dd>
<dt>argument block</dt>
<dd>A block element that is a direct child of a parent tag.</dd>
<dt>parameter block</dt>
<dd>A block element that is not a direct child of a parent tag.</dd>
</dl>

## Standalone tags and elements

Mustache tries to avoid adding undesired blank lines for non-interpolation tags.
For example, assuming that `boolean = true`, the following template:

```mustache
| This Is
{{#boolean}}
|
{{/boolean}}
| A Line
```

will render as:

```plain
| This Is
|
| A Line
```

instead of:

```plain
| This Is

|

| A Line
```

A tag is considered **standalone** if the tag is the only text on the line except whitespace.
Interpolation tags are never considered standalone.
The leading whitespace, trailing whitespace, and line ending of a standalone tag are ignored.

Additionally, parent elements and parameter blocks
are considered standalone if the start tag has zero or more whitespace characters preceding it on its line
and its associated end tag has zero or more whitespace characters following it on its line.
In such cases, the leading whitespace of such a parent tag or parameter block's start tag,
the trailing whitespace of the associated end tag,
and the line ending of the associated end tag are ignored.

## Indentation

Mustache tries to keep indentation consistent when using partials, parents, and blocks.
For example, for the following template:

```mustache
{{<parent}}{{$block}}
    one
    two
{{/block}}{{/parent}}
```

and the following definition of `parent`:

```mustache
Hi,
  {{$block}}
  {{/block}}
```

the template will render as:

```plain
Hi,
  one
  two
```

instead of:

```plain
Hi,
      one
      two
```

If a block tag is at the end of a line (ignoring any trailing whitespace)
and it is either part of an argument block or part of a standalone parameter block,
then the block element's **intrinsic indentation** is the sequence of leading whitespace characters on the line following the block tag.
Otherwise, the block element's intrinsic indentation is the empty string.
Mustache implementations must ignore the occurrence of the block element's intrinsic indentation
at the beginning of each line of the block element's content.

When a standalone partial tag or standalone parent element is expanded,
the leading whitespace on the start tag's line is prepended to each line of the expanded template.
A similar behavior occurs for standalone parameter blocks.
If a standalone parameter block's start tag is at the end of a line (possibly with trailing whitespace),
then its intrinsic indentation is prepended to each line of the argument block that is overriding it.
Otherwise, the leading whitespace of the standalone parameter block's start tag
is prepended to each line of the argument block that is overriding it.
