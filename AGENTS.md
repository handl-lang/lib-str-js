# lib-str-js

JavaScript implementation of the `str` ports via the `js` ffi
(`js.call`/`js.invoke` against `String`/`Array`/`RegExp`). Pure `.hnl`,
single module (`src/index.hnl`, all 45 `str/<arrow>` contract
signatures).

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | all 45 contract arrows in one module |
| `src/index.test.hnl` | the .hnl test suite |
| `pkg.hnl` | manifest — `modules: ["str-js"]`, dep `str` only (`js` is a host/builtin contract, NOT a dep — see Contract) |
| `scripts/hnl-gate.sh`, `workflows/gate.yaml` | the gate / bupha entry |

## Contract

- Unlike `str-c`/`str-handl`, this is a **single module** — import
  from `"str-js"`; `str-js/<arrow>` submodule paths do not exist.
- Native JS methods cover the encoding-insensitive surface; everything
  char-indexed goes through `_chars` (`Array.from` — code points, not
  UTF-16 units) plus pure-handl list ops ported from `str-handl`, so
  results agree with the reference on astral input.
- `find`/`rfind` convert JS's UTF-16 offsets back to code-point
  indices; `replace` is literal `join(split(...))` (no `$`-pattern
  surprises); `is_*` classifiers use `\p{...}` Unicode property
  escapes. List values marshal via the `js_object` descriptor.
- `js` stays out of `pkg.hnl` `deps` and `.handl.toml`
  `[dependencies]`: the `js` contract is a host/builtin surface
  (compile-js binds `from "js"` to `__handl_rt` shims regardless of
  dep tables). Declaring the dep makes `hnl run`/`hnl test` resolve
  `js` to `lib-js`'s signature-only exports — `call` erases to `null`
  and every call dies with "callee is not an arrow-value". Undeclared,
  the import lowers to the ambient `"js.call"` callee and fails
  honestly as `call_host: function 'js.call' not found` on hosts
  without JS natives (the shape `scripts/hnl-gate.sh` tolerates).
- `index.hnl`'s header lists the full divergence list (JS trim set vs
  `White_Space`, context-sensitive whole-string case conversion,
  per-char ffi cost on composed paths).

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
