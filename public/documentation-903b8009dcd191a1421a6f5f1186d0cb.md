# Documentation

This chapter describes how the documents have been made and how to maintain them.

## Documents to be maintained

We have the following documents, including this book, to maintain.

|No |   Book Name           | Book License | Used Tool | Version |
|:-:|:--------------------------|:--------|:-----------|:---------|
| 1 |[Molass Legacy Reference](https://biosaxs-dev.github.io/molass-legacy/) |GPL-3.0 |Sphinx | - |
| 2 |[Molass Libray Reference](https://biosaxs-dev.github.io/molass-library/)|GPL-3.0 |Sphinx | - |
| 3 |[Molass Libray Essence](https://biosaxs-dev.github.io/molass-essence/)  |CC BY 4.0|Jupyter Book| v2 (MyST) |
| 4 |[Molass Libray Tutorial](https://biosaxs-dev.github.io/molass-tutorial/)|CC BY 4.0|Jupyter Book| v2 (MyST) |
| 5 |[Molass Technical Report](https://biosaxs-dev.github.io/molass-technical/)|CC BY 4.0|Jupyter Book| v2 (MyST) |
| 6 |[Molass Developer's Handbook](https://biosaxs-dev.github.io/molass-develop/)|CC BY 4.0|Jupyter Book| v2 (MyST) |

For the first two reference books, we use [Sphinx](https://github.com/sphinx-doc/sphinx) directly to generate function documents from their [docstrings](https://peps.python.org/pep-0257/). For others, [Jupyter Book v2 (MyST)](https://mystmd.org/) is used.

## How to use Jupyter Book v2 (MyST)

```{note}
Do not confuse "Jupyter Notebook" and "Jupyter Book". The former is a file for programming, while the latter is a tool for publishing. As of 2026, we use **Jupyter Book v2**, which is based on MyST (Markedly Structured Text) and uses the `myst` command-line tool.
```

### Tool Package Installation

To use Jupyter Book v2 (MyST), you need to install the following package.

```
pip install mystmd
```

```{note}
Jupyter Book v2 uses MyST (Markedly Structured Text) as its core engine. The command-line tool is `myst` rather than `jupyter-book`. Books created with v1 need migration (see the GitHub Actions workflows in each repository for automated deployment).
```

### Initial Book

Each initial book was created as follows.

    ・ myst init --yes
    ・ create an empty repository for book-repo on GitHub
    ・ git clone book-repo
    ・ copy the generated files to book-repo
    ・ create myst.yml with project title, GitHub link, and template settings

```{note}
You need this procedure only when you create a new book. For v2 details, see [MyST documentation](https://mystmd.org/guide). The `myst init` command creates a basic project structure.
```

### Manual Book Edit

When you begin to edit source files manually, the main configuration file is:

    ・ myst.yml

For compatibility, you may also have:

    ・ _config.yml (legacy, v1)
    ・ _toc.yml (legacy, v1)

The `myst.yml` file is the primary configuration for v2. It defines the project metadata, site template, and build options. As you edit the source file, which is either a markdown text file like "coding_style.md" or a Jupyter notebook like "prepare.ipynb", you should be well acquainted with the syntax summarized in [MyST syntax guide](https://mystmd.org/guide/syntax-overview).

```{note}
Syntaxes of markdown texts differ slightly depending on the tools they are processed with. Be sure to follow the `MyST syntax` here. On the other hand for GitHub texts like `README.md`, follow [Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).
```

See also as you proceed:

* [MyST Documentation](https://mystmd.org/guide)
* [MyST Syntax Overview](https://mystmd.org/guide/syntax-overview)
* [Jupyter Book v1 to v2 migration notes](https://mystmd.org/)

```{tip}
For common issues like duplicate titles in notebooks, see [Common Issues and Troubleshooting](#common-issues-and-troubleshooting) below.
```

### Book Update Cycle

So far, we have described the intial procudure required only once when you create a new book. Atfer that,
we repeat the following cycle to update, brief descriptions of which will follow below.

    ・ edit manually
    ・ generate locally
    ・ synchronize master (or main) branch
    ・ deploy gh-pages branch

```{note}
For maintenance of the web book, the two branches, namely main and gh-pages, are involved. The former keeps the source and the latter the generated book. With GitHub Actions deployment, the gh-pages branch is managed automatically. We use the name "main" for the primary branch.
```

After manual edit, local generation should be achieved as follows in Command Prompt:

```
cd book-repo
myst build --html
```

For testing with the GitHub Pages base URL (to match production), use:

**Windows PowerShell:**
```
$env:BASE_URL = "/repo-name"
myst build --html
```

**Linux/macOS:**
```
BASE_URL=/repo-name myst build --html
```

Replace `repo-name` with the actual repository name (e.g., `/molass-tutorial`). This ensures asset links work correctly when deployed to GitHub Pages at `https://biosaxs-dev.github.io/repo-name/`.

After generation, check the generated local output in `_build/html` with your browser. Re-edit as needed until you are satisfied.

:::{admonition} Clean builds
:class: tip
If you need to force a clean rebuild, delete the `_build` directory first:

```
rm -rf _build
myst build --html
```

MyST v2 handles caching differently than v1. Clean builds are rarely needed.
:::

Synchronization of the main branches, local and remote, should be achieved as follows in VS Code:

    ・ commit in VS Code
    ・ synchronize with GitHub in VS Code

### Deployment via GitHub Actions

All Jupyter Book repositories now use **automated deployment via GitHub Actions**. When you push to the main branch, the workflow automatically:

1. Installs `mystmd`
2. Runs `myst build --html`
3. Deploys to GitHub Pages

See `.github/workflows/deploy.yml` in each repository for the workflow configuration.

### Manual deployment (alternative)

If you need to deploy manually, you can still use `ghp-import`:

```
ghp-import -n -p -f _build/html
```

The web page will be updated in a few minutes. However, **GitHub Actions deployment is preferred** as it ensures consistency and handles the build environment automatically.

### Common Issues and Troubleshooting

#### Duplicate Titles in Notebooks

**Problem**: A notebook page shows duplicate titles (e.g., "Installation" appearing twice on the same page)

**Root cause**: MyST derives the page title from the **first H2 heading** if no explicit title is set. For example:

```markdown
# Quick Start
## Installation
```

MyST ignores the H1, uses "Installation" as the page title, AND renders "## Installation" as content → duplication.

**Solution**: Add MyST frontmatter to the **first markdown cell** of the notebook:

````markdown
---
title: Quick Start
---

# Quick Start
## Installation
...
````

This explicitly sets the page title, preventing MyST from deriving it from the first H2 heading.

**Testing locally**:
1. Save the notebook
2. `myst start` → opens server at http://localhost:3000
3. Navigate to the affected page
4. Verify no duplication
5. Ctrl+C to stop server

**Reference**: Fixed in molass-tutorial commit dd4da49 (2026-06-29)

## How to use Sphinx

If you are just updating existing parts of the document, skip to [Usual Update Routine](#usual_update_routine).

### Tool Package Installation

To use Sphinx for API documentation, install the following packages:

```
pip install sphinx
pip install sphinx-book-theme
pip install sphinx-copybutton
pip install myst-parser
```

```{note}
**Sphinx is used only for API documentation** (molass-library, molass-legacy), not for the documentation books. The books now use MyST v2 (`mystmd`), which has no Sphinx dependency. The `myst-parser` package is the Sphinx extension for parsing MyST markdown in API docs.
```

### Initial Generation

When you are updating, skip to [Gererate *.rst files](#generate_rst_files).

To generate a set of initial examples, execute commands as follows in the repository root.

```
mkdir docs
cd docs
sphinx-quickstart
```

Reply minimally, i.e. defaults or none, to queries from the last command except the following two. 
* Project name: `Molass Library`
* Author name(s): `Molass Community`

The command will gerenate several files, among which you should edit the follow two. 

* `conf.py`
* `index.rst`

We will give some hints on edition later.

### Customize `conf.py`

* Add the root directory to the system path to ensure the documentation of the current state
* Add extensions
* Set sphinx_book_theme

(generate_rst_files)=
### Gererate *.rst files

The following command line will generate *.rst files from Python source codes in ../molass folder.
Namely in docs:

```
sphinx-apidoc --output-dir source ../molass --separate --module-first
```

```{note}
For [Molass Legacy Reference](https://biosaxs-dev.github.io/molass-legacy/), replace `molass` with `molass_legacy`.
```

After this generation, the folder tree will look as follows.

```
    molass-library/
        docs/
            _static/
            source/
            tools/
            make.bat
            Makefile
            conf.py
            index.rst
        molass/
```

We confined *.rst files into the "source" subfolder to make it clear that they should be generated from the python source codes in "molass" folder, and as such, they should be ignored in version control of Git. (See `.gitignore` to confirm this)

### Edit `index.rst`

The *.rst files can be made into HTML files only if referenced. In oeder to be referenced, we must initiate the reference chain of HTML files in `index.rst`

* edit `index.rst`

That is, if the HMTL generator `sphinx-build`, invoked later in a `make` command line, finds `source/molass.Baseline` in the `index.rst`, thenn it looks into it and finds a next reference, and so on.

### Modify *.rst files

Unfortunately, we have to modify the first version of *.rst files generated by `sphinx-apidoc` in the above step because they include redundantly repeated prefixes or modifiers, like `molass.`, `package` or `module`, in titles which we will remove by the following command line.

In docs:

```
python tools/EditRst.py
```

This script edits all .rst files in the "docs/source" directory to update the title lines.

### Make HTML files

Now we are ready to invoke `sphinx-build` by the following command line.

In docs:

```
make html
```

This will produce HTML files in "_build" subfolder.

### Deployment

The following command line will deploy them as we did with `Jupyter Book` above.

In docs:

```
ghp-import -n -p -f _build/html
```

This (re)creates the gh-pages branch both in local and remote repositories, and the web book will be updated in a few minutes.

(usual_update_routine)=
### Usual Update Routine

After editing parts of source code, do the following in docs. 

```
python tools/UsualUpdate.py
```

See the [module documentation](https://biosaxs-dev.github.io/molass-library/source/UsualUpdate.html#module-UsualUpdate) for details.