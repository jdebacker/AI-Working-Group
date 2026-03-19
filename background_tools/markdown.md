# Markdown

## What is Markdown?

Markdown is a lightweight **plain-text formatting syntax** that converts to HTML (and many other formats). It was designed to be readable as-is — even before rendering — while still producing clean formatted output.

Markdown is relevant in research contexts because:

- Most AI tools (including ChatGPT and Claude) output responses in Markdown
- GitHub renders Markdown automatically in READMEs, issues, and pull requests
- Jupyter notebooks, Quarto documents, and Jupyter Book (which powers this site) all use Markdown for prose
- Many academic writing tools (Obsidian, Zotero notes, VS Code) support Markdown natively

Learning Markdown takes about 15 minutes and pays dividends across many tools.

## Basic Syntax

### Headings

Use `#` symbols to create headings. More `#` symbols = smaller heading.

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
```

### Emphasis

```markdown
*italic* or _italic_
**bold** or __bold__
***bold and italic***
~~strikethrough~~
```

Renders as: *italic*, **bold**, ***bold and italic***, ~~strikethrough~~

### Lists

**Unordered list** (bullet points):

```markdown
- Item one
- Item two
  - Nested item
  - Another nested item
- Item three
```

**Ordered list**:

```markdown
1. First step
2. Second step
3. Third step
```

### Links and Images

```markdown
[Link text](https://example.com)
[Link with title](https://example.com "Hover text")

![Alt text](path/to/image.png)
![Alt text](https://example.com/image.png)
```

### Inline Code and Code Blocks

Use backticks for inline code: `` `variable_name` `` renders as `variable_name`.

For a block of code, use triple backticks. Optionally specify the language for syntax highlighting:

````markdown
```python
import pandas as pd

df = pd.read_csv("data.csv")
print(df.head())
```
````

### Blockquotes

```markdown
> This is a blockquote.
> It can span multiple lines.
>
> And multiple paragraphs.
```

### Horizontal Rule

```markdown
---
```

### Tables

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Value A  | Value B  | Value C  |
| Value D  | Value E  | Value F  |
```

Renders as:

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Value A  | Value B  | Value C  |
| Value D  | Value E  | Value F  |

Column alignment can be controlled with colons:

```markdown
| Left | Center | Right |
|:-----|:------:|------:|
| A    |   B    |     C |
```

## Extended Syntax

Many Markdown renderers support extensions beyond the core spec.

### Task Lists

```markdown
- [x] Completed task
- [ ] Incomplete task
- [ ] Another task
```

### Footnotes

```markdown
Here is a claim.[^1]

[^1]: This is the footnote text.
```

### Math

Many Markdown environments (including Jupyter Book) support LaTeX math:

```markdown
Inline math: $y = \beta_0 + \beta_1 x + \varepsilon$

Display math:
$$
\hat{\beta} = (X'X)^{-1}X'y
$$
```

### Callout Boxes (Jupyter Book / MyST)

Jupyter Book uses **MyST Markdown**, which adds directives for callout boxes:

````markdown
```{note}
This is a note callout.
```

```{warning}
This is a warning callout.
```

```{tip}
This is a tip callout.
```
````

## Markdown Flavors

There are several variants of Markdown. The most common ones you will encounter:

| Flavor | Used In |
|--------|---------|
| CommonMark | Standard baseline spec |
| GitHub Flavored Markdown (GFM) | GitHub READMEs, issues, PRs |
| MyST Markdown | Jupyter Book, Sphinx |
| Pandoc Markdown | Pandoc conversions, Quarto |
| R Markdown | RStudio / Quarto |

The differences are mostly in extended features (tables, footnotes, math). Core syntax is the same across all flavors.

## Markdown in Practice

### README Files

Every GitHub repository should have a `README.md`. It is the first thing visitors see and is automatically rendered on the repository's homepage.

### Jupyter Notebooks

In a Jupyter notebook, "Markdown" cells render formatted text alongside code cells. This is a powerful way to combine narrative, equations, and analysis in one document.

### Quarto and R Markdown

[Quarto](https://quarto.org/) is a newer document system that supports Markdown with embedded R, Python, or Julia code. It is increasingly popular for reproducible academic papers and reports.

## Further Resources

- [Markdown Guide](https://www.markdownguide.org/) — comprehensive reference with basic and extended syntax
- [CommonMark Spec](https://commonmark.org/) — the standardized Markdown specification
- [MyST Markdown Documentation](https://mystmd.org/) — the extended syntax used in Jupyter Book
- [GitHub Markdown Guide](https://docs.github.com/en/get-started/writing-on-github) — GitHub-specific Markdown features
