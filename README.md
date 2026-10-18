# lib-str-js

JavaScript implementation of the `str` ports (via the js ffi).

Part of the `handl-lang` org split (ADR-0128). This repo is a **skeleton**:
the layout and gate are in place, the implementation has not landed yet.

## Layout

```text
src/        the .hnl sources (empty until the implementation lands)
pkg.hnl     dependencies, when any: `{ deps: { <name>: "branch:<ref>" } }`
```

Dependencies: str

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

CI skips the gate while `src/` has no `.hnl` sources.
