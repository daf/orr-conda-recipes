# orr-conda-recipes
Recipes for building conda packages for many (all?) of the third party packages required by ORR tools -- specifically GNOME

This repo should give you all you need to build conda pacakges for various third party tools needed by py_gnome. IN theoery, these will be built by ORR developers, and provided on binstar in the NOAA-ORR-ERD channel. But you you want to build yourself, this is how to do it.

# Geting set up

To build them all, you can use Phil Elson's lovely obvious-ci scripts:

https://github.com/pelson/Obvious-CI

First you will need to install obvios-ci itself:

conda install obvious-ci

or, if you are doing this totally from scratch, you can built it yourself

conda build obvious-ci

# Obvious CI
[Obvious-CI](https://github.com/pelson/Obvious-CI) is a collection of tools that help with continuous integration like [travis-ci](https://travis-ci.org/ioos/conda-recipes/builds) and [AppVeyor](https://ci.appveyor.com/project/comtbot/conda-recipes/history).

In this case, we're using it on our develpment machines, but it's very helpful

Using Obvious-CI, the single script `obvci_conda_build_dir.py` can build all binaries in this repository, ordered by their dependency.  Note that `obvci_conda_build_dir.py` reads the packages that are already on the channels and does not build if they are the same.

Here's how to get the script working on your system: 

```bash
conda config --add channels NOAA-ORR-ERD -f

conda install obvious-ci

git clone https://github.com/NOAA-ORR-ERD/orr-conda-recipes.git

obvci_conda_build_dir.py ./conda-recipes NOAA-ORR-ERD --channel main
```

The last command will build everything in the directory we just cloned against the `NOAA-ORR-ERD` channel.  It is highly recommended that you remove any other channel from your conda config (e.g.: `conda config --remove channels ChrisBarker -f`) to avoid conflicts. You may even want to setup a new miniconda environment to get a really clean system.

If you are a NOAA-ORR-ERD binstar admin, or you are building for your own personal repository, you may want to upload the packages as you build.  The `obvci_conda_build_dir.py` script will do that for you if you set a `BINSTAR_TOKEN`:

```bash
TOKEN=$(binstar auth -n NAME-OF-YOUR-TOKEN --max-age 22896000 -c --scopes api)
export BINSTAR_TOKEN=$TOKEN
```

