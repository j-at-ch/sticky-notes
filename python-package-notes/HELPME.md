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

Once correctly configured, tests should be runable via
`pytest tests`. The command `pytest` will do the work of
discovering all nested tests provided that standard naming
conventions have been followed.

### 