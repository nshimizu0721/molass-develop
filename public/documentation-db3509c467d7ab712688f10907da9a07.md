# Documentation

This chapter describes how the documents have been made and how to maintain them.

## Documents to be maintained

We have the following documents, including this book, to maintain.

|No |   Book Name           | Book License | Used Tool |
|:-:|:--------------------------|:--------|:-----------|
| 1 |[Molass Legacy Reference](https://biosaxs-dev.github.io/molass-legacy/) |GPL-3.0 |Sphinx |
| 2 |[Molass Libray Reference](https://biosaxs-dev.github.io/molass-library/)|GPL-3.0 |Sphinx |
| 3 |[Molass Libray Essence](https://biosaxs-dev.github.io/molass-essence/)  |CC BY 4.0|Jupyter Book|
| 4 |[Molass Libray Tutorial](https://biosaxs-dev.github.io/molass-tutorial/)|CC BY 4.0|Jupyter Book|
| 5 |[Molass Technical Report](https://biosaxs-dev.github.io/molass-technical/)|CC BY 4.0|Jupyter Book|
| 6 |[Molass Developer's Handbook](https://biosaxs-dev.github.io/molass-develop/)|CC BY 4.0|Jupyter Book|

For the first two reference books, we use [Sphinx](https://github.com/sphinx-doc/sphinx) directly to generate function documents from their [docstrings](https://peps.python.org/pep-0257/). For others, [Jupyter Book](https://github.com/jupyter-book/jupyter-book), which depends on Sphinx, is used.

## How to use Jupyter Book

```{note}
Do not confuse "Jupyter Notebook" and "Jupyter Book". The former is a file for programming, while the latter is a tool for publishing.
```

### Tool Package Installation

To use Jupyter Book, you need to install the following two packages.

```
pip install jupyter-book==1.0.4.post1
pip install -U ghp-import
```

```{note}
The latest version of jupyter-book will be later than version 2.0.0, which is incompatible with the existing contents of the repository created with jupyter-book version 1. 
```

### Initial Book

Each initial book was created as follows.

    ・ jupyter-book create example-repo
    ・ create an empty repository for book-repo on GitHub
    ・ git clone book-repo
    ・ copy the contents of example-repo to book-repo

```{note}
You need this procedure only when you create a new book. In case of need, see [Create your first book](https://jupyterbook.org/en/stable/start/your-first-book.html) for details.
```

### Manual Book Edit

When you begin to edit source files manually, to be attended first are the following two.

    ・ _config.yml
    ・ _toc.yml

These files are usually placed either at the root of repository or a sub folder named like "docs". Compare them with those of an existing book you already have, then you will get to know which source files to edit. As you edit the source file, which is either a markdown text file like "coding_style.md" or a Jupyter notebook like "prepare.ipynb", you should be well acquainted with the syntax summarized in [MyST syntax cheat sheet](https://jupyterbook.org/en/stable/reference/cheatsheet.html).

```{note}
Syntaxes of markdown texts differ slightly denending on the tools they are processed with. Be sure to follow the `MyST syntax` here. On the other hand for GitHub texts like `README.md`, follow [Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).
```

See also as you proceed:

* [Create your first book](https://jupyterbook.org/en/stable/start/your-first-book)
* [MyST syntax cheat sheet](https://jupyterbook.org/en/stable/reference/cheatsheet.html)

### Book Update Cycle

So far, we have described the intial procudure required only once when you create a new book. Atfer that,
we repeat the following cycle to update, brief descriptions of which will follow below.

    ・ edit manually
    ・ generate locally
    ・ synchronize master (or main) branch
    ・ deploy gh-pages branch

```{note}
For maintenance of the web book, the two branches, namely master and gh-pages, are involved. The former keeps the source and the latter the generated book. Instead of the word "master", "main" might be used depending on how the repository was created. Anycase, be sure to use the word consistently. We use the name "master" here in this chapter.
```

After manual edit, local generation should be achieved as follows in Command Prompt:

```
cd book-repo
jupyter-book build .
```

After generation, check the generated local output in _build/html with your browser. Re-edit as needed until you are satisfied.

:::{admonition} A note on page caching
:class: tip
Sometimes, you may need `--all` option. That is,

```
jupyter-book build --all .
```

See [a note on page caching](https://jupyterbook.org/en/stable/start/build.html#aside-source-vs-build-files) for details.
:::

Synchronization of the master branches, local and remote, should be achieved as follows in VS Code:

    ・ commit in VS Code
    ・ synchronize with GitHub in VS Code

(Re)creation and synchronization of the gh-pages branches, local and remote, should be achienved as follows in Command Prompt:

```
ghp-import -n -p -f _build/html
```

The web page will be updated in a few minutes, as a result of automatic deployment of GitHub Pages.
If you are interested in details of this step, see the [README](https://github.com/c-w/ghp-import) of ghp-import.

## Temporary Migration v1 → V2 Memo

On Windows PowerShell

```
cd c:\Users\takahashi\GitHub\molass-develop
$env:BASE_URL = "/molass-develop"
myst build --html

ghp-import -n -p -f _build/html
```

## How to use Sphinx

If you are just updating existing parts of the document, skip to [Usual Update Routine](#usual_update_routine).

### Tool Package Installation

To use Sphinx, you need to install the following two packages, which should have been installed if you have already installed packages for Jupyter Book as stated above.

```
pip install sphinx~=7.0 
pip install sphinx-book-theme
pip install sphinx-copybutton
```

```{note}
The version specification ~=7.0 to shpinx comes from Jupyter Book's dependecy constraints. Later versions than this would cause troubles with Jupyter Book as of April, 2025.
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