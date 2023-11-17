# python-package-notes

### Testing via `pytest`
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

```
project-template    # Project folder
├── conf            # Configuration files
├── data            # Local project data
├── docs            # Documentation
├── logs            # Logs of pipeline runs
├── notebooks       # Exploratory Jupyter notebooks 
├── pyproject.toml  # Project metadata and linter configuration
├── README.md       # README.md explaining your project
└── src             # Source code for pipelines
```

`pytest` suggests a slight divergence from this with `tests` outside of `src` rather than within. 

### Resources:

+ `pytest` good integration practices. [Here.](https://pytest.org/en/7.4.x/explanation/goodpractices.html)
+ A useful data science template from kedro. [Here.](https://docs.kedro.org/en/stable/get_started/kedro_concepts.html#kedro-project-directory-structure)
+ Configuring a `setuptools` build via `pyproject.toml`. [Here.](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html)
+ Loading `dotenv` without altering environ. [Here.](https://pypi.org/project/python-dotenv/)
+ Pytest modifying traceback. [Here.](https://docs.pytest.org/en/7.1.x/how-to/output.html)
  + Recommend `pytest -tb=no` as a starting point.
+ A blog post on testing python packages. [Here.](https://hynek.me/articles/testing-packaging/) 

