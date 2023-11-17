# python-package-notes

### Testing via `pytest`
Resources:
+ Good testing integration practices: [pytest](https://pytest.org/en/7.4.x/explanation/goodpractices.html)
+ A blog with extra tips: [here](https://blog.ionelmc.ro/2014/05/25/python-packaging/#the-structure%3E)

My experiences:
+ If there is a ModuleNotFoundError for the source code,
then test whether it works using `python -m pytest tests`.
If so it's possible that the `tests` module is missing `__init__.py`
at each of its levels, or that the package itself wasn't
properly installed using `pip install -e .` and a `setup.py`.

Once correctly configured, tests should be runnable via
`pytest tests`. The command `pytest` will do the work of
discovering all nested tests provided that standard naming
conventions have been followed.

### Adding packages to a conda env's `PATH`.

I found [this resource](https://towardsdatascience.com/python-and-the-module-search-path-e71ae7a7e65f), which
points out that local package paths can be added via adding `.pth` files to the `site-packages` dir as 
`package_name.pth`, containing the path to that package. This works, but, e.g. some non PyPI packages that 
I've seen seem to be added to the path automatically. I'm not sure why this is.

### Project templates:

kedro suggests [this](https://docs.kedro.org/en/stable/kedro_project_setup/starters.html):

project-template    # Project folder
├── conf            # Configuration files
├── data            # Local project data
├── docs            # Documentation
├── logs            # Logs of pipeline runs
├── notebooks       # Exploratory Jupyter notebooks 
├── pyproject.toml  # Project metadata and linter configuration
├── README.md       # README.md explaining your project
└── src             # Source code for pipelines

`pytest` suggests a slight divergence from this with `tests` outside of `src` rather than within. 

