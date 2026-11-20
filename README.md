# qasida-tmLanguage

[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Version 1.0.0](https://img.shields.io/badge/version-1.0.0-blue)](package.json)
[![Scope: source.qasida](https://img.shields.io/badge/scope-source.qasida-lightgrey)](syntaxes/qasida.tmLanguage.json)

TextMate grammar for [Qasida](https://github.com/themythofsisyphusjayant/RED-MOON-)
(`.fg` files, scope `source.qasida`). This is the syntax grammar
[github/linguist](https://github.com/github/linguist) vendors to highlight
Qasida on GitHub.

## Contents

- `syntaxes/qasida.tmLanguage.json` — the grammar (24 top-level patterns)
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

### Scopes

The 24 top-level patterns assign these scopes:

| Scope | What it highlights |
|---|---|
| `comment.block.qasida` | `/* ... */` block comments |
| `comment.line.double-slash.qasida` | `//` line comments |
| `string.quoted.double.qasida` | Double-quoted strings, with `constant.character.escape.qasida` for `\` escapes |
| `keyword.control.qasida` | All 27 keywords; also the `fn`/`extern`/`struct`/`enum`/`trait`/`impl`/`type`/`newtype`/`scope` in declaration positions |
| `entity.name.function.qasida` | Function name after `fn` / `extern fn` |
| `entity.name.type.qasida` | Type name after `struct` / `enum` / `trait` / `impl` / `type` / `newtype` (including both sides of `impl Trait for Struct`) |
| `storage.type.qasida` | Builtin types (`int` … `u64`, `never`, `void`, `Option`, `Result`) |
| `support.function.qasida` | Builtins |
| `constant.language.qasida` | `true` / `false`, and the wildcard `_` |
| `constant.numeric.*.qasida` | `hex`, `binary`, `octal`, `float`, `integer` literals |
| `keyword.operator.qasida` | Operators |
| `punctuation.qasida` | Punctuation |

## Install and use in VS Code

The manifest (`package.json`) declares the language `qasida` for `.fg`
files, requires VS Code `^1.85.0`, and points at the grammar and the
language configuration. To install from source:

```sh
git clone https://github.com/themythofsisyphusjayant/qasida-tmLanguage.git
mkdir -p ~/.vscode/extensions/qasida-1.0.0
cp -r qasida-tmLanguage/{package.json,language-configuration.json,syntaxes} \
  ~/.vscode/extensions/qasida-1.0.0/
```

Then restart VS Code and open any `.fg` file — it is recognised as Qasida
and highlighted with the scopes above. (On macOS the extensions folder is
`~/.vscode/extensions/` as well; on Windows it is
`%USERPROFILE%\.vscode\extensions\`.)

`language-configuration.json` also gives the editor its behaviour for
`.fg` files: `//` line comments and `/* */` block comments, brackets
`{}` `[]` `()`, auto-closing pairs for all three plus `"` (not inside a
string), surrounding pairs, indent increase on a line ending in `{` and
decrease on a line starting with `}`, and a word pattern covering numbers
with decimal points and identifiers.

### Example

```qasida
fn main() -> i32 {
    let mut total = 0
    for n in 0..10 {
        total = total + n
    }
    // sum 0..9
    return total
}
```

`fn`, `let`, `mut`, `for`, `in`, `return` highlight as control keywords;
`i32` as a builtin type; `0..10` as integers with the `..` range operator;
`// sum 0..9` as a line comment.

## Origin

Derived from the grammar shipped in the Qasida VS Code extension in
[RED-MOON-](https://github.com/themythofsisyphusjayant/RED-MOON-/blob/main/editors/vscode/qasida/syntaxes/qasida.tmLanguage.json),
updated to the v2.0.0 lexer (`forge_lang/lexer.py`, `forge_lang/checker.py`):
8 missing keywords, fixed-width int types, hex/binary/octal literals, `<<`
`>>` `..` operators, and the newer builtins were added.

## License

MIT — see [LICENSE](LICENSE).
