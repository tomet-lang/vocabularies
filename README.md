<!-- Generated from tmtroot/readme.tmt. Edit that, then `tomet export .`. -->

# Vocabularies

Versioned Tomet vocabularies, shared by the repositories of the ecosystem.

## Overview

A vocabulary is a `@vocabulary(name){ version: ... }` document that names the elements a kind of document may use. Repositories take the vocabularies they need from here with `catasto`, pinned to a commit and locked by SHA-256, so every repository reads the same text and drift shows up as a failed `catasto check` instead of a silent difference.

## Layout

```text
vocabularies/<name>/<name>@<version>.vocabulary.tmt
```

- `<name>` is the vocabulary's namespace, as `@vocabulary(<name>)` declares it.
- `<version>` is the full semantic version the file declares. Pre-release versions are not published.

A file is exact; a document asks loosely. A document names only the major version it was written for (`@kind(writ, 1)`, `@use(rust, 1)`), because it is a user of the vocabulary and any compatible release serves it. The vocabulary file is the thing itself, so its name carries the whole version: two releases of the same major differ in content.

The name stays the same when a repository takes the file, so a repository can hold several versions of one vocabulary side by side. A name without `@version`, like `deck.vocabulary.tmt`, is a vocabulary the repository owns and edits itself.

## Released versions are immutable

A file that has been pushed is never edited, not even to fix a typo: a fix is a new version beside it, so a lock that names a path always reads the bytes it was made from. A breaking change is a new major version; adding an element is a minor version; anything else is a patch.

## Included

- `writ` -- the rules a repository obeys, written in `.writ.tmt`. Owned by `twrit`.
- `rust` -- references to Rust items in documents, such as `@rust.crate(name)`. General purpose.

## Taking a vocabulary

Declare where the file comes from in the repository's `catasto.ctmt`, then run `catasto sync`:

```text
"tmtroot/schema/vocabularies/writ@1.0.0.vocabulary.tmt": {
  source: { git: "https://github.com/tomet-lang/vocabularies.git", rev: main, path: "vocabularies/writ/writ@1.0.0.vocabulary.tmt" }
}
```

Pin `rev` to a tag or a commit once you depend on it.

## License

Dual-licensed under the MIT license and the Apache License 2.0, at your option.

