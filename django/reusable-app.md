##### A folder must contain a __init__.py to be considered a python package. Use setuptools to build the package.

1. move /myapp to a new folder /django-myapp outside the project folder

2. in /django-myapp: create LISENSE(usually BSD license), README.rst, setup.py(details about how to build and install the app), MANIFEST.in(include non-python files) and /docs

3. build the package from /django-myapp:
```
python setup.py sdist
```
-> new package inside /dist

##### install the package
```
pip install --user django-myapp/dist/django-myapp-0.1.tar.gz
```
Exclude --user when inside virtualenv!

And:
```
pip uninstall django-myapp
```
