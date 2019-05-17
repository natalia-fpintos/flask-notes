# Flask tutorial - Web application

## Start here

To begin with, in the command line, `cd` into the root of your project folder. We will need to create a virtual environment for Python, and install Flask as a dependency:

```
$ cd my-flask-application

# We create a virtual environment called "flask-venv" with the standard library venv solution
$ python3 -m venv flask-venv

# We activate the virtual environment, so we can start working on it
$ . venv/bin/activate

# We install Flask as a dependency
$ pip install Flask
```
<br/>

## My first Flask application

We can create a very simple application with a single python file:

```python
from flask import Flask

# __name__ will be the name of the current python module
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'
```

This will create a flask app which will return a "Hello World" page when we access the main route.
<br/>

## Modules and Packages

As applications grow bigger, rather than keeping them in a single file, we would create a python `modules` and `packages` instead.

* __Module:__ a module is simply a `python file`, which usually contains code that fulfills a specific functionality. The name of the module is the name of the file which contains this code.

* __Package:__ a package is a `folder` that contains modules and maybe other packages as well. This package acts as a namespace when importing modules, being the name of the package (or the namespace) the name of the folder itself. The only requirement to turn a folder into a package is that the folder must contain an `__init__.py` file. This file doesn't need to contain any code, it simply indicates that the folder that contains the file is a package and it can be imported in the same way as a module.
<br/>

## Creating a flask application

A Flask application is an instance of the `Flask` class, which we import from the `flask` package.

We can create this application in different ways:

1. As the example above, we can simply create an `app` variable which will contain the instance of the `Flask` class.
2. We can create an `application factory` which will create the `Flask` instance, do any required setup, and then return this instance.

```python
import os

from flask import Flask

def create_app(test_config=None):
    # Create and configure the app
    # "instance_relative_config=True" indicates that configuration files are relative to the 'instance folder', which is outside the app
    app = Flask(__name__, instance_relative_config=True)

    # Sets the config with the keys and values provided
    app.config.from_mapping(
        SECRET_KEY='dev',
        DATABASE=os.path.join(app.instance_path, 'flaskr.sqlite'),
    )

    if test_config is None:
        # Overrides the config with the values in config.py, if in the instance folder
        app.config.from_pyfile('config.py', silent=True)
    else:
        # Load the test config if passed in
        app.config.from_mapping(test_config)

    # Ensure the instance folder exists
    try:
        os.makedirs(app.instance_path)
    except OSError:
        pass

    # A page in the '/hello' route
    @app.route('/hello')
    def hello():
        return 'Hello, World!'

    return app
```
<br/>

## Running the application

In order to run the application, we firstly need to tell Flask where the application is, and we can also tell it to run it in dev mode.
Then we can use `flask run` to start the application.

Make sure your `cwd` is one level above the package for the application (i.e. above `my-flask-app`).

```
$ export FLASK_APP=my-flask-app
$ export FLASK_ENV=development
$ flask run
```

At this point, going to the `http://127.0.0.1:5000/hello` page should display a 'Hello, World!' message.
