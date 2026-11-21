# Karkain-CLI

Command-line application construction helpers for the Karkain ecosystem.

- **Language baseline:** Karkain Language v1.2.0 (verified tag `v1.2.0`,
  commit `77d70693e1e3134dac312a303bffafa7d50fa988`).
- **Framework version:** `0.1.0-dev`.
- **Status:** VERIFIED — core implemented, 10 native tests, integration run
  and a compiled native binary consuming real process arguments.
- **Repository model:** independent repository; no submodules.

## Purpose

Karkain ships its own compiler CLI; this framework does not replace or
modify it. Karkain-CLI provides small, portable primitives for the argument
surface of a Karkain command-line program: token classification, command and
option extraction, positional access, command lookup and usage formatting.

## Public API

Module: `src/kcli.kark` (import as `kcli`).

| Function | Purpose |
|---|---|
| `process_args()` | Return the real process argument vector (`getArgs()`). |
| `is_option(token)` | Whether a token starts with `-`. |
| `command(args)` | First token after the program name, or `""`. |
| `has_flag(args, flag)` | Whether an exact flag token is present. |
| `option_value(args, name)` | Token following `name`, or `""`. |
| `positional_count(args)` | Count of non-option tokens after the program name. |
| `positional(args, n)` | nth non-option token, or `""` when out of range. |
| `command_index(commands, cmd)` | Index of `cmd`, or `-1` when unknown. |
| `usage(program, commands)` | Build a single-line usage string. |
| `describe_command(name, description)` | Build a help line. |

## Layout

```text
src/kcli.kark           framework implementation (public API)
src/verify.kark         integration program
tests/api_tests.kark    10 native test_* bodies
examples/cli_demo.kark  runnable native example
scripts/verify.ps1      verification gate
COMPATIBILITY.md        evidence-based baseline assessment
karkain.toml            project manifest (version 0.1.0-dev)
```

## Verification

```powershell
powershell -ExecutionPolicy Bypass -File scripts\verify.ps1
```

Gates: module `check`; 10 native tests (expected 10/10); integration `run`;
a negative gate proving exit code 4 on failure; a native `build` of the
example followed by execution with `greet --name Ajit --verbose`.

## Dependencies

None. Only verified Karkain v1.2.0 builtins are used.

## Known limitations

- `karkain run` does not forward extra arguments; use `karkain build` and run
  the produced executable for real argument input (verified).
- The parser is deliberately minimal: option values are not distinguished
  from positional tokens. Use `option_value` for named options.
- `karkain test` cannot resolve imports, so tests are generated from the real
  module source plus test bodies in a temporary file.
- No cross-platform or heterogeneous-target support is claimed. See
  `COMPATIBILITY.md`.
