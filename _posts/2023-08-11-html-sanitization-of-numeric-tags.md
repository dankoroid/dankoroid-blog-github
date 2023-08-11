---
title: "HTML sanitization on example of numeric tag"
date: 2023-08-11
---

Watched a cool video about HTML sanitization - [Generic HTML Sanitizer Bypass Investigation](https://www.youtube.com/watch?v=HUtkW2gjC8Q) on [LiveOverflow YT channel](https://www.youtube.com/@LiveOverflow) about some example that seems weird at first:
```
<22>test</22>
which results in
<22>test
```
Reason - `<22>` is not a valid HTML tag, because it can't start from non-ASCII alpha character.

There is a definition of HTML parse error in HTML specification - [invalid-first-character-of-tag-name](https://html.spec.whatwg.org/#parse-error-invalid-first-character-of-tag-name)

> This error occurs if the parser encounters a code point that is not an ASCII alpha where first code point of a start tag name or an end tag name is expected.


So, what are the possible implications?
Some custom-written HTML parsers could potentially match <22> as valid HTML tag:

```<22 foo="bar<img src=x onerror=alert(1)>">test<22>```

As this would trigger JS - and could potentially be used for inappropriate behavior.
