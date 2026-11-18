# Jira wiki markup

Use this syntax in Jira descriptions. Jira does not render Markdown.

## Headings

Start the line with `hN. ` where N is 1 to 6:

```
h1. Biggest heading
h3. Section heading
h6. Smallest heading
```

## Text effects

```
*strong*
_emphasis_
-deleted-
+inserted+
{{monospaced}}
bq. A one-paragraph block quote.
{quote}
A quote longer than one paragraph.
{quote}
```

Use `{{monospaced}}` for code identifiers, commands, file paths, and config keys.

## Breaks

- An empty line starts a new paragraph.
- `\\` forces a line break. It is rarely needed.
- `----` on its own line draws a horizontal rule.
- `---` renders as an em dash and `--` as an en dash.

## Links

```
[https://example.com]
[Link label|https://example.com]
[mailto:someone@example.com]
```

Brackets are optional for a bare URL. Separate any character that follows a URL but is not part of it with a space.

## Lists

Lists must start in the first column.

Bulleted, with `**` for a nested level:

```
* item
** nested item
* item
```

Numbered, with `##` for a nested level:

```
# first
# second
## nested
```

Mixed:

```
# step
#* bullet under the step
# next step
```

## Tables

`||` marks header cells and `|` marks regular cells:

```
||Heading 1||Heading 2||
|A1|A2|
|B1|B2|
```

## Preformatted text and code

```
{noformat}
Text shown as is, with *no* formatting applied.
{noformat}

{code:json}
{"key": "value"}
{code}
```

`{code}` without a language defaults to Java. Supported languages: ActionScript, Ada, AppleScript, bash, C, C#, C++, CSS, Erlang, Go, Groovy, Haskell, HTML, Java, JavaScript, JSON, Lua, Nyan, Objc, Perl, PHP, Python, R, Ruby, Scala, SQL, Swift, VisualBasic, XML, YAML.

## Escaping

`\X` renders the character X literally, for example `\[`, `\{`, `\*`.

## Common mistakes

- Markdown syntax: `**bold**`, `# Heading`, `[text](url)`, and triple-backtick fences do not render. A line starting with `# ` becomes a numbered list item.
- Square brackets: `[anything]` becomes a link. Escape literal brackets as `\[` and `\]`, or put the text in `{{ }}`.
- Curly braces: `{word}` can start a macro. Escape as `\{` or use `{{ }}` / `{code}`.
- Paired symbols: text between `*`, `_`, `-`, `+`, `^`, or `~` at word edges changes formatting, for example `-flag-` renders as deleted text. Escape the symbol or use `{{ }}`.
- Dashes: `--` and `---` in prose become en and em dashes. Write `\-\-` to keep two literal dashes, or put CLI flags in `{{ }}`.
- Exclamation marks: text between two `!` can render as an image. Escape as `\!`.
- Emoticons: sequences such as `(y)`, `(x)`, `(!)`, `(i)`, `:)`, and `;)` render as icons. Escape the parenthesis or use `{{ }}`.
- Pipes: `|` inside a table cell splits the cell. Escape as `\|`.
