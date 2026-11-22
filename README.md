# lib-str-js

JavaScript implementation of the `str` ports (via the `js` ffi —
`js.call`/`js.invoke` against `String`/`Array`/`RegExp`).

`src/index.hnl` implements all 45 `str/<arrow>` contract signatures as a
single module. Native JS methods cover the encoding-insensitive surface
(`toUpperCase`/`toLowerCase`/`trim*`, `includes`/`startsWith`/`endsWith`,
non-empty-separator `split`, `repeat`, `join`, whole-string case maps);
everything char-indexed goes through `_chars` (`Array.from` — code
points, not UTF-16 units) plus pure-handl list ops ported from
`str-handl`, so `length`/`char_at`/`slice`/`find`/`rfind`/`count`/
`lines`/the case converters/`levenshtein`/`similarity`/`slugify`/
`word_wrap` etc. agree with the reference on astral input.
`find`/`rfind` convert JS's UTF-16 offsets back to code-point indices;
`replace` is literal `join(split(...))` (no `$`-pattern surprises);
`is_*` classifiers use `\p{...}` Unicode property escapes.

Unlike `str-c`/`str-handl` this is a **single module** — import from
`"str-js"`; `str-js/<arrow>` submodule paths do not exist.

See the `index.hnl` header for the full divergence list (JS trim set vs
`White_Space`, context-sensitive whole-string case conversion, the
`js_object` descriptor for list values, per-char ffi cost on the
composed paths).

## Layout

```text
src/index.hnl       the .hnl sources (all 45 contract arrows)
src/index.test.hnl  contract tests mirroring lib-str-handl's suites
pkg.hnl             package manifest — modules, deps, description, license
```

Dependencies: str, js

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.
