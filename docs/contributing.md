# Contributing Guide

Thank you for being interested in contributing to the `sphinx-book-theme`! You
are awesome ✨.

This project follows the Executable Books Project [contribution guidelines](https://executablebooks.org/en/latest/contributing.html).
It contains information about our conventions around coding style, pull request workflow, commit messages and more.

This page contains information to help you get started with development on this
project.

## Set up your development environment

1. Get the source code of this project using git:

   ```bash
   git clone https://github.com/executablebooks/sphinx-book-theme
   cd sphinx-book-theme
   ```

2. Ensure you have Python 3.6 or newer!
3. Install `tox`.
   `tox` is a tool for managing virtual environments for test suites or common jobs that are run with a repository.
   It ensures that your environment is consistent each time you build the docs.

   ```console
   $ pip install tox
   ```

   You can list all of the `tox` environments like so:

   ```console
   $ tox -a
   ```

   Each one corresponds to a test environment or a task to build the documentation, and has its own virtual environment.

   See the `sphinx-book-theme/tox.ini` for details of all available development environments.
4. Install `pre-commit`.
   We use pre-commit to ensure that the code meets certain standards any time a commit is made.
   First, [follow the `pre-commit` installation instructions](https://pre-commit.com/#install).
   Then, run the following command in the same folder as the repository:

   ```console
   $ pre-commit install
   ```

   :::{note}
   You can also run pre-commit via `tox`:
   ```console
   $ tox -e py38-pre-commit -- --all
   ```
   or manually run all `pre-commit` jobs for this repository:

   ```console
   $ pre-commit run --all-files
   ```
   :::


### Preview changes you make to the theme

If you're making changes to your local copy of this theme, you can get feedback on the rendered documentation output using `tox` jobs.
These will preview this theme's documentation using the latest changes you've made.

To build the documentation, run these commands:

```console
$ tox -e docs-clean
$ tox -e docs-update
```

This will generate the HTML documentation and compile the relevant stylesheets.
The generated documentation which can be viewed in `docs/_build/`.

Alternatively, you can have the documentation built "live", as you modify files:

```console
$ tox -e docs-live
```

This will do the following:

- Generate the theme's documentation (similar to running `tox -e docs-update`)
- Start a development server on an available port `localhost:xxxx`
- Open it in your default browser
- Watch for changes and automagically regenerate the documentation and auto-reload your browser when a change is made.

With this, you can modify the theme in an editor, and (after a small delay) see
how those modifications render on the browser.

:::{admonition} To use different Sphinx builders

You can tell Sphinx to build the documentation with different builders like so:

```console
$ tox -e docs-update -- singlehtml
$ tox -e docs-live -- singlehtml
```
:::

### Running Tests

This theme has a test suite to ensure that all the relevant user content is
correctly handled. The tests can be run using:

```console
$ tox
```

You can also run against different Python and sphinx versions, and supply arguments to pytest:

```console
$ tox -e py38-sphinx2 -- -x
```

:::{tip}
To "re-build" an environment, wih updated dependancy versions, use:

```console
$ tox -r -e py38-sphinx2
```

:::

## Repository structure

This repository is a split into a few critical folders:

sphinx-book-theme/
: The Sphinx extension package, containing the Python code, Jinja templates and (compiled) assets (HTML/CSS/JS etc).
: These are used to generate the HTML page for every file in your site whenever the site is built using Sphinx.
: **NOTE**: Do not alter the compiled CSS/JS directly (alter in `src/`).

src/
: Contains the source SCSS and JS, which will be compiled and written to `sphinx-book-theme/static`, as configured by `web-compile-config.yml` (see [executablebooks/web-compile](https://github.com/executablebooks/web-compile) for details).
: This compilation is called by default, during development commands (see above).

docs/
: The documentation for the theme, which aims to include all of the core Sphinx "components" (lists, admonitions, etc), to check/show how they are represented in this theme.
: The build configuration is contained in `conf.py`.

tests/
: Testing infrastructure that uses `pytest` along with `beautifulsoup` to validate
  that the generated HTML is what we expect it to be.
: Much of these tests also use `pytest-regressions`, to check whether newly-generated HTML differs from previously-generated HTML.

.github/workflows/
: Contains Continuous-integration (CI) workflows, run on commits/PRs to the GitHub repository.
