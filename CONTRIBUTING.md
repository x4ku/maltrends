# Contributing Instructions

Instructions for contributing to this project.

## Setup

Create and activate a virtual environment:
```sh
$ python -m venv env
$ source env/bin/activate
```

Install dependencies:
```sh
$ pip install -r requirements-dev.txt
```

## Development

Lint the source code with `flake8`:
```sh
$ flake8 bin/*
```

Run the unit tests:
```sh
$ bin/test
```

## Updating Data Files

The `bin/update` script uses the [MAL API (v2)]. You will need to register for
this API and obtain a Client ID in order to run this script. This Client ID is
passed to the script via the `MAL_CLIENT_ID` environment variable.

See: https://myanimelist.net/apiconfig

Update the data files:
```sh
$ MAL_CLIENT_ID=... bin/update
```

<!-- links -->

[MAL API (v2)]: https://myanimelist.net/apiconfig/references/api/v2
