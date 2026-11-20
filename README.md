# qasida-tmLanguage

[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Version 1.0.0](https://img.shields.io/badge/version-1.0.0-blue)](package.json)

TextMate grammar for [Qasida](https://github.com/themythofsisyphusjayant/RED-MOON-)
(`.fg` files, scope `source.qasida`). This is the syntax grammar
[github/linguist](https://github.com/github/linguist) vendors to highlight
Qasida on GitHub.

## Contents

- `syntaxes/qasida.tmLanguage.json` — the grammar
- `package.json` — VS Code extension manifest declaring the grammar (the
  form Linguist's `script/add-grammar` reads)
- `language-configuration.json` — brackets, comments, auto-closing pairs

## Coverage

The grammar matches the Qasida v2.0.0 lexer: all 27 keywords (`fn let mut
if else while for in return true false import as struct enum match spawn
extern break continue trait impl type newtype opaque readonly defer`),
builtin types (`int float bool str range i8 i16 i32 i64 u8 u16 u32 u64 never
void Option Result`), `//` and `/* */` comments, double-quoted strings with
escapes, decimal/hex (`0x`)/binary (`0b`)/octal (`0o`) numbers with `_`
separators and float exponents, operators (`== != <= >= && || -> => << >>
..` and friends), `fn` / `struct` / `enum` / `trait` / `impl` (including
`impl Trait for Struct`) / `type` / `newtype` / `extern fn` declarations,
all builtins, and constants (`true`/`false`, `_`).

There are no char literals in Qasida (a lone `'` is a lexer error), so the
grammar does not define one. `scope` is an identifier in the lexer, used as
the structured-concurrency block opener (`scope { ... }`); the grammar
highlights it as a control keyword in that position only.

Qasida has moved on since v2.0.0, and this grammar has not been re-checked
against later releases — syntax added after v2.0.0 may not be highlighted
until the grammar is updated.

## Origin

Derived from the grammar shipped in the Qasida VS Code extension in
[RED-MOON-](https://github.com/themythofsisyphusjayant/RED-MOON-/blob/main/editors/vscode/qasida/syntaxes/qasida.tmLanguage.json),
updated to the v2.0.0 lexer (`forge_lang/lexer.py`, `forge_lang/checker.py`):
8 missing keywords, fixed-width int types, hex/binary/octal literals, `<<`
`>>` `..` operators, and the newer builtins were added.

## License

MIT — see [LICENSE](LICENSE).
