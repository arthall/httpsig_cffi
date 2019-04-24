# README_dev.md

## Using pyenv

https://github.com/pyenv/pyenv-installer

Follow the instructions and add the pyenv setting to you profile

Once your shell has been restarted, install the version of python you want to use.
You can get a list from `pyenv install --list`
I installed Python 3.7.3, Python 3.5.7, Pypy 2.7-7.1.1 and Pypy3.6-7.1.1.  I could not get
Python 3.4.9 installed due to issues with openssl.

It appears that Pop!_OS 18.04 LTS needs a number of additional libraries installed to get
Python to build.  See the common build problems page for pyenv

https://github.com/pyenv/pyenv/wiki/common-build-problems

I needed the following installed before it build build Python3 without errors or warnings.

    zlib1g-dev
    libffi-dev
    libreadline-dev
    libbz2-dev
    libssl-dev
    libsqlite3-dev

Once all the dependencies are installed, you should be able to run

``` bash
pyenv install 3.7.3
```

You can verify the new version of Python is installed with 

``` bash
pyenv versions
```

Set Python3 as the version in use and setup a virtual environment

``` bash
pyenv shell 3.7.3
pyenv virtualenv httpsig
pyenv sell httpsig
```

In the virtual environment run

``` bash
pip install -r requirements.txt
```

My environment has the following

``` bash
pip list
Package      Version
------------ -------
asn1crypto   0.24.0 
cffi         1.12.3 
cryptography 2.6.1  
pip          19.0.3 
pycparser    2.19   
setuptools   40.8.0 
six          1.12.0 
```

## Tox and Pyenv

Tox needs to be able to find the various versions of Python but it doesn't understand how pyenv organizes them.

The easist way I found to do this is to set the versions needed by tox as global in pyenv

``` bash
pyenv global system 3.7.3 3.5.7 pypy2.7-7.1.1 pypy3.6-7.1.1
```

You have to be out side of the virtual environment when running tox.

My tox environment is: `export TOXENV=py27,py35,py36,py37,pypy,pypy3` because I don't have Python 3.2, 3.3 or 3.4 installed.

Inside the pyenv-virtualenv I run `tox -e py37`

## Flake8

To keep things stylisticall consitent, the project uses Flake8
