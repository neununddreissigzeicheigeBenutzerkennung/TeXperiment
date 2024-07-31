This repository is focused around the TeX typesetting engine. It mainly contains plain old `.tex` files in the folder `userpackages`.

## Installation & Usage

In addition, there is a `PKGBUILD` file which can be used to create a `tar.gz` package which can be installed by `pacman` under Arch Linux.

For those unfamiliar, you can create the package by running:

```
$ makepkg
```

in the folder containing the `PKGBUILD`.

The resulting package `texperiment*.tar.gz` can then be installed using:

```
$ pacman -U texperiment*.tar.gz
```

This will install the packages into your TeX Live installation so you can simply include them by `\input`ing them. It also creates a format which can be loaded by the TeX compiler to have all packages available at once.

```
$ tex -fmt=atexperiment(.fmt) filename.tex
```

Take note that this doesn't work with different TeX compilers like `pdftex` or `latex`. If this presents an issue, you can run:

```
$ dvipdf filename.dvi
```

to create a PDF file or make your own adjustments to the TeX Live installation.

The package also creates a custom shell script so you can run TeX with the format preloaded more easily:

```
$ atex filename
```

yields the same result as manually loading the format file `atexperiment.fmt`.

## Documentation

All of the TeX packages are documented even if not visible at first glance. "It’s safely buried where no one will ever stumble upon it — hopefully, including myself!"

To obtain a package's documentation, it is as simple as calling the TeX compiler on it. In reality, it is not quite as simple as made out to be, however.

The TeX compiler first needs access to the required packages, including the package whose documentation you desire as well as the package used to document it. This is coincidentally one of the packages in this repository and can thus be installed via `pacman` using the `PKGBUILD`.

If this is not possible because you are not running Arch Linux for some bizarre reason or you simply don't want to install all the bloat, feel free to take and install just what you want. You should, however, keep in mind that many of the packages are codependent.

If the installation of the required packages was successful, the acquisition of a package's documentation is as simple as running:

```
$ tex packagename.tex
```

miraculously generating the package's documentation wherever you are. If you are interested in more details, the author suggests looking at the documentation of the package `documentation.tex`.

## Custom Installation

To install `userpackages` into a TeX Live installation, you first need to move them to the correct folder. This folder can be located using the command:

```
$ kpsewhich -var-value=TEXMFDIST
```

which is the `texmf-dist` distribution's path for TeX and METAFONT. There you should choose the path `tex/generic`. The author suggests following this by `/texperiment` for easy relocation of the packages.

Once you have copied the desired files, you need to tell TeX about the changes you made. This is done by running:

```
$ mktexlsr "the/path"/texmf-dist
```

If you aren't using all packages, make sure you don't leave out any dependencies.

If you want to create a format file, you can do this by first creating a file `formatname.tex`, in which you `\input` all the desired packages and then running:

```
$ initex formatname.tex
```

thus creating `formatname.fmt`. To install this package, it needs to be moved to the correct path again, in this case `texmf-dist/web2c/tex/formatname.fmt`. Remember to apply changes using:

```
$ mktexlsr "the/path"/texmf-dist
```

Creating a "custom compiler" script is as easy as creating the correctly named file in `/usr/bin` and adding two lines of code:

```bash
#!/bin/
tex -fmt=atexperiment "$@"
```

Finally, make the script executable:

```
$ chmod +x
```

and you are caught up to the `PKGBUILD`.
