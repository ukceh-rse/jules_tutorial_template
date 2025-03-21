# JULES tutorial template

## Requirements

- A Unix-based operating system
- A Bash shell
- `curl`
- `git` version 2.13 or higher


## Setup

Clone the repository using the following command,

```sh
git clone --recurse-submodules https://github.com/ukceh-rse/jules_tutorial_template
```

This also clones the [`portable-jules` repository](https://github.com/jmarshrossney/portable-jules) as a [submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

### JULES setup

Follow the 'Quickstart' instructions at [github.com/jmarshrossney/portable-jules](https://github.com/jmarshrossney/portable-jules/blob/main/README.md) (ignoring the step that clones the repository) to download and build JULES.

Once you have successfully installed JULES and run the 'Loobos' example successfully, return to the repository root directory with `cd ..`.

> [!IMPORTANT]
> If you ran `devbox shell` at any time, make sure you have exited it.
> This should be obvious from the absence of `📦` in your prompt.
> You can also run `which sh` and check that it points to your usual shell, **not** `/nix/store/<stuff>/bin/sh`.

### Python setup

I recommend using [`pyenv`](https://github.com/pyenv/pyenv) to install and manage Python.
Run the following commands _in the repository root_ to set up Python 3.12 on your system,

```sh
# Install pyenv
curl -fsSL https://pyenv.run | bash

# Add pyenv to your $PATH
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc
. ~/.bashrc

# Install Python 3.12 using pyenv
pyenv install 3.12

# Set python3.12 as the default Python version in this repository
pyenv local 3.12
```

Now create a [virtual environment](https://docs.python.org/3/library/venv.html) and install the required packages with `pip`,

```sh
# Create a virtual environment in the .venv/ directory
python -m venv .venv

# Activate the virtual environment
source .venv/bin/activate

# Install the packages listed in requirements.txt
python -m pip install -r requirements.txt
```


## Jupyter notebooks

> [!IMPORTANT]
> Make sure you have activated the virtual environment before running these commands.

We first need to build the notebooks from the corresponding markdown files.
To do this, run

```sh
jupytext --to ipynb experiment/notebook.md
```

To launch Jupyter Lab in the `experiment/` directory, run

```sh
jupyter lab experiment/
```


## Development

Feel free to fork this repository and develop your own tutorials.

### portable-jules

Keep this up to date.

```sh
cd portable-jules/
git pull
cd ..
```

### Packages

Please update `requirements.in` with any new packages, and then run

```sh
pip-compile requirements.in
```

to regenerate `requirements.txt`.

It is a good idea to do this second step periodically to keep existing packages up to date.

### Notebooks

If you edit a notebook and want to commit changes to git, you need to sync it with the markdown version,

```sh
jupytext sync experiment/notebook.ipynb
```

You should see that `experiment/notebook.md` has been updated to mirror the `ipynb` version (check by running `git diff experiment/notebook.md`).


## Extensions

- Could include some `R` notebooks, or blend the two using Quarto instead of Jupyter.
