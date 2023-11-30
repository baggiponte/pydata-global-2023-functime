# How I Used Polars to Build functime, a Next Gen ML Forecasting Library

[![cookiecutter slidev](https://img.shields.io/badge/cookiecutter-slidev-D4AA00?logo=cookiecutter&logoColor=fff)](https://github.com/baggiponte/cookiecutter-slidev)

Source code for the talk *How I Used Polars to Build functime, a Next Gen ML Forecasting Library* at PyData Global 2023.

- 📽️ [Live]()

## 🛩️ How to run

Make sure you have installed the following:

* `git` (of course)
* GitHub CLI: `gh` (optional)
* `node.js`
* `just` is a command runner (optional, recommended for contributing)

1. Clone the repo

```bash
# with github CLI
gh repo clone baggiponte/pydata-global-2023-functime

# with git
git clone git@github.com:baggiponte/pydata-global-2023-functime
```

> **Note**
>
> 🎬 How to configure SSH
>
> Cloning over SSH is safer. Follow [this guide](https://www.youtube.com/watch?v=5o9ltH6YmtM).
> Alternatively, you can follow the steps in [this](https://github.com/git-merge-workshops/simplify-signing-with-ssh/blob/main/exercises/01-setup-workstation.md) workshop of GitHub's.


2. Install the dependencies

```bash
# with the command runner
just install

# with npm
npm install
```

3. Open the slideshow locally

```bash
# with the command runner
just preview

# with npx
npx slidev --open -- slides.md
```

4. Visit http://localhost:3030

## 🤗 contributing

Install the dependencies [above](,/README.md#%EF%B8%8F-how-to-run) (`just` is highly recommended) and the following:

* [`commitizen`](https://commitizen-tools.github.io/commitizen/) is a release management tool. It's used to release new versions.

1. Fork the repo
2. Create your own branch.
3. Before opening a PR, run the following:

```bash
just test-release
```

This will format the slides, try to build them and test whether a version bump is possibile.

> **Note**
>
> Credits:
>
> - [`slidev`](https://github.com/slidevjs/slidev) is an amazing framework to build slides from markdown and host them.
> - [`just`](https://github.com/casey/just) is just a command runner.
> - [`commitizen`](https://commitizen-tools.github.io/commitizen/) is a release management tool.
