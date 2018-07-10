---
title: How to submit a recipe to conda-forge
date: July 10th, 2018
---


# conda-forge

It is a community led collection of recipes,
build infrastructure,
and packages for the `conda` package manager.


# The problem we are trying to solve

```shell
$ conda search my-awesome-pkg

PackagesNotFoundError: The following packages are not available from current channels:

  - my-awesome-pkg

```


# We want to avoid this scenario!

![xkcd 1654](images/universal_install_script.png)


# conda-skeleton to the rescue

```shell
conda skeleton pypi my-awesome-pkg

or

conda skeleton cran my-awesome-pkg
```


. . .
```
To use 'conda skeleton', install conda-build.
```

# Recipe sections: `package` and `source`

```yaml
{% set version = "1.0.0" %}

package:
  name: my-awesome-pkg
  version: {{ version }}

source:
  url: https://URL/my-awesome-pkg-{{ version }}.tar.gz
  sha256: 410bcd1d6409026fbaa65d9ed33bf6dd8b1e94a499e32168acf
```


# Recipe sections: `build` and `requirements`

```yaml
build:
  number: 0
  noarch: python
  script: python -m pip install --no-deps --ignore-installed .

requirements:
  build:
    - python
    - pip
  run:
    - python

```

# Recipe sections: `test`

```yaml
test:
  imports:
    - my-awesome-pkg
  commands:
    - my-awesome-pkg-cli --help
```


# Recipe sections: `about` and `extra`


```yaml
about:
  home: http://my-awesome-pkg.org/
  license: Public Domain
  summary: 'Dummy package'
  license_family: OTHER

extra:
  recipe-maintainers:
    - ocefpaf
```


# Build the recipe locally or with CIs

>- `docker pull condaforge/linux-anvil`
>- `conda build my-awesome-pkg`
>- enable Travis-CI and AppVeyor on your fork


# Submit the recipes to conda-forge

>- submit a PR to [https://github.com/conda-forge/staged-recipes](https://github.com/conda-forge/staged-recipes)
>- request a review from the stage-recipes team
>- that's it


# What happens next...

>- the recipe will be built for OS X (Travis-CI), Linux (CircleCI), and Windows (AppVeyor)
>- the community volunteers will review and merge the recipe
>- once merged the recipe will be removed from `staged-recipes`
>- a new repository, feedstock, will be created and the recipe maintainers will have write access to it


# Maintaining a package

>- the maintainer is responsible updating, reviewing and merging PRs to the feedstock
>- some of those PRs will be automatic making the maintainer life easier


# Hands on exercise!

>- install conda-build
>- choose a package from CRAN or PyPI
>- run the `conda-skeleton` command for the package
>- adapt the recipe for conda-forge
>- submit a PR to staged-recipes
