# Mesa Sphinx
A simple port of the [mesa3d.org](mesa3d.org) website from HTML to reStructured Text.
This repository is using the read the docs theme.

## Instalation
To use this repository you need to have the latest [Python](https://www.python.org/) and [Sphinx](http://www.sphinx-doc.org/en/stable/) software installed.
The readTheDocs theme must be installed too.
You can install it using pip:

    pip install sphinx_rtd_theme

## View the generated site
After everything is installed, simply issue:

    cd cloned_repository_folder
    make html

If everything is setup correctly, this will create a folder called `build/html` with all the website content.


To view the website at local machine you can go directly to `build/html/index.html`.

