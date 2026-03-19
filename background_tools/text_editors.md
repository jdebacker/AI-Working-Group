# Text Editors

## Why Your Editor Matters

A good text editor is the central hub of a modern research workflow. You write code, prose, and configuration files in it; you run terminals from it; and increasingly, you interact with AI assistants through it. Investing a little time in choosing and configuring an editor pays off substantially.

This page covers the most relevant options for economists doing research with code and AI tools.

## VS Code (Recommended Starting Point)

[Visual Studio Code](https://code.visualstudio.com/) is currently the most widely used editor in research and data science contexts. It is free, open source, and developed by Microsoft.

**Key strengths:**

- Excellent Python support (syntax highlighting, linting, autocomplete, debugging)
- Integrated terminal
- Built-in Git integration — stage, commit, and view diffs without leaving the editor
- Jupyter notebook support (run `.ipynb` files directly in the editor)
- Large extension ecosystem
- Native GitHub Copilot and other AI assistant integrations
- Available on macOS, Windows, and Linux

**Recommended extensions for economists:**

| Extension | Purpose |
|-----------|---------|
| Python (Microsoft) | Python language support, linting, debugging |
| Pylance | Enhanced Python type checking and autocomplete |
| Jupyter | Run Jupyter notebooks in VS Code |
| GitHub Copilot | AI code completion |
| GitLens | Enhanced Git history visualization |
| Markdown All in One | Markdown editing and preview |
| Stata Enhanced | Syntax highlighting for Stata `.do` files |
| R (REditorSupport) | R language support |

**Getting started:** Download from [code.visualstudio.com](https://code.visualstudio.com/), then open a folder with `File > Open Folder`. The integrated terminal can be opened with `` Ctrl+` `` (or `` Cmd+` `` on macOS).

## Cursor

[Cursor](https://cursor.com/) is a fork of VS Code that has deep AI integration built in from the ground up, rather than added via extension. It looks and feels almost identical to VS Code — the same interface, the same extensions, the same keyboard shortcuts — but AI features are first-class citizens.

**Key AI features:**

- **Tab completion** — context-aware multi-line code suggestions
- **Cmd+K** — edit a selected block of code with a natural language instruction
- **Cmd+L / Chat panel** — chat with Claude or GPT-4 with full awareness of your codebase
- **`@` mentions** — reference specific files, functions, or documentation in your chat prompt
- **Composer** — make coordinated edits across multiple files at once

Cursor is arguably the most productive environment for AI-assisted coding currently available. Because it is a VS Code fork, any VS Code user can switch with essentially zero learning curve.

**Pricing:** Free tier available; Pro plan (~$20/month) for heavier use.

## JupyterLab and Jupyter Notebook

[JupyterLab](https://jupyter.org/) is a browser-based interactive computing environment centered on Jupyter notebooks. It is the standard tool for exploratory data analysis in Python and R.

**Key strengths:**

- Notebooks interleave code, output (including figures), and Markdown prose
- Ideal for exploratory analysis and communicating results
- Supports Python, R, Julia, and other kernels
- JupyterLab (the newer interface) adds a file browser, multiple panels, and a terminal

**Limitations compared to VS Code:**

- Less suitable for editing standalone `.py` scripts
- No built-in Git GUI (though a Git extension exists)
- Not ideal for large multi-file projects

**Best used for:** exploratory data analysis, sharing results as a narrative document, teaching.

JupyterLab is typically launched from the terminal:

```bash
jupyter lab
```

## RStudio / Positron

[RStudio](https://posit.co/products/open-source/rstudio/) remains the dominant editor for R users. It has deep integration with R, R Markdown, and Quarto for document authoring.

[Positron](https://positron.posit.co/) is Posit's newer IDE (built on VS Code) that supports both R and Python more uniformly. It is worth watching as it matures.

**Best used for:** R-heavy workflows, Quarto documents, mixed R/Python projects.

## Vim / Neovim

[Vim](https://www.vim.org/) and its modern fork [Neovim](https://neovim.io/) are terminal-based editors with a modal editing model (different modes for navigation, insertion, and commands). They have a steep learning curve but are extremely fast for experienced users and work on any remote server over SSH.

AI integration is available via plugins like [Avante.nvim](https://github.com/yetone/avante.nvim) or [CopilotChat.nvim](https://github.com/CopilotC-Nvim/CopilotChat.nvim).

**Best used for:** users who already know Vim, remote server work, terminal-only environments.

## Emacs

[Emacs](https://www.gnu.org/software/emacs/) is a decades-old, highly extensible editor with a devoted following in academia. The [Doom Emacs](https://github.com/doomemacs/doomemacs) and [Spacemacs](https://www.spacemacs.org/) distributions provide opinionated configurations that modernize it considerably.

**Best used for:** existing Emacs users; not recommended for newcomers.

## Comparison Summary

| Editor | Best For | AI Integration | Learning Curve |
|--------|----------|----------------|----------------|
| VS Code | General use, Python, notebooks | GitHub Copilot, extensions | Low |
| Cursor | AI-first coding | Built-in (best-in-class) | Low (VS Code fork) |
| JupyterLab | Exploratory analysis, notebooks | Extensions | Low |
| RStudio / Positron | R, Quarto, mixed R/Python | Copilot extension | Low–Medium |
| Vim / Neovim | Speed, remote servers | Plugins | High |
| Emacs | Extensibility, existing users | Plugins | High |

## Recommendation

For most economists getting started:

1. **Install VS Code** as your primary editor for scripts and multi-file projects
2. **Try Cursor** once you are comfortable — the AI features are genuinely useful and the transition is seamless
3. **Use JupyterLab** for exploratory data analysis and sharing notebooks

## Further Resources

- [VS Code Documentation](https://code.visualstudio.com/docs)
- [Cursor Documentation](https://cursor.com/docs)
- [JupyterLab Documentation](https://jupyterlab.readthedocs.io/)
- [VS Code Python Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
