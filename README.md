![python](https://cloud.githubusercontent.com/assets/51578/13712821/b68a42ce-e793-11e5-96b0-d8eb978137ba.png)

# Heroku Buildpack: Python

[![Build Status](https://travis-ci.org/heroku/heroku-buildpack-python.svg?branch=master)](https://travis-ci.org/heroku/heroku-buildpack-python)

This is the official [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [Pipenv](http://docs.pipenv.org/), [pip](https://pip.pypa.io/) and other excellent software.

Recommended web frameworks include **Django** and **Flask**. The recommended webserver is **Gunicorn**. There are no restrictions around what software can be used (as long as it's pip-installable). Web processes must bind to `$PORT`, and only the HTTP protocol is permitted for incoming connections.

Python packages with C dependencies that are not [available on the stack image](https://devcenter.heroku.com/articles/stack-packages) are generally not supported, unless `manylinux` wheels are provided by the package maintainers (common). For recommended solutions, check out [this article](https://devcenter.heroku.com/articles/python-c-deps) for more information. 

See it in Action
----------------

Deploying a Python application couldn't be easier:

    $ ls
    Pipfile		Pipfile.lock	Procfile	web.py

    $ heroku create --buildpack heroku/python

    $ git push heroku master
    …
    -----> Python app detected
    -----> Installing python-3.6.6
    -----> Installing pip
    -----> Installing requirements with Pipenv 2018.5.18…
           ...
           Installing dependencies from Pipfile…
    -----> Discovering process types
           Procfile declares types -> (none)

A `Pipfile` or `requirements.txt` must be present at the root of your application's repository.

You can also specify the latest production release of this buildpack for upcoming builds of an existing application:

    $ heroku buildpacks:set heroku/python


Specify a Python Runtime
------------------------

Specific versions of the Python runtime can be specified in your `Pipfile`:

    [requires]
    python_version = "2.7"

Or, more specifically:

    [requires]
    python_full_version = "2.7.15"

Or, with a `runtime.txt` file:

    $ cat runtime.txt
    python-2.7.15

Runtime options include:

- `python-3.7.0`
- `python-3.6.6`
- `python-2.7.15`

## Tests

The buildpack tests use [Docker](https://www.docker.com/) to simulate
Heroku's [stack images.](https://devcenter.heroku.com/articles/stack)

To run the test suite:

```
make test
```

Or to test in a particular stack:

```
make test-heroku-18
make test-heroku-16
```

The tests are run via the vendored
[shunit2](https://github.com/kward/shunit2)
test framework.

## Releasing

To create a release:

- Rebase the last three commits onto `heroku/heroku-buildpack-python`
    ```shell
    git remote rm heroku || true
    git remote add heroku https://github.com/plotly/heroku-buildpack-python
    git rebase heroku master
    git push -f origin master
    ```
- Create a tag. This tag should correspond with the current herokuish release to indicate what release we're building on top of.
    ```shell
    # if latest gliderlabs/herokuish is 0.4.5
    git tag 0.4.5-1
    git push --tags
    ```
- CI will build a release and push to quay.io
