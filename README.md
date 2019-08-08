Buildout for Farmandat Site Development
=======================================

This buildout creates a suitable environment for Farmandat Site. It installs a Substance D application.


Installing system dependencies
------------------------------

First of all install or check Python build dependencies.

  $ sudo apt-get update; sudo apt-get install --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev


Setting up virtualenv
---------------------

Install pyenv to choose Python 3.5.7. Read full instructions from https://github.com/pyenv/pyenv.git
Check out pyenv where you want it installed. A good place to choose is $HOME/.pyenv

  $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv

Define environment variable PYENV_ROOT to point to the path where pyenv repo is cloned and add $PYENV_ROOT/bin to your $PATH for access to the pyenv command-line utility.

  $ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
  $ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

Proxy note: If you use a proxy, export http_proxy and HTTPS_PROXY too.
Add pyenv init to your shell to enable shims and autocompletion. Please make sure eval "$(pyenv init -)" is placed toward the end of the shell configuration file since it manipulates PATH during the initialization.

  $ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc

General warning: There are some systems where the BASH_ENV variable is configured to point to .bashrc. On such systems you should almost certainly put the abovementioned line eval "$(pyenv init -)" into .bash_profile, and not into .bashrc. Otherwise you may observe strange behaviour, such as pyenv getting into an infinite loop.

Restart your shell so the path changes take effect. You can now begin using pyenv.

  $ exec "$SHELL"

Install Python versions into $(pyenv root)/versions. For example, to download and install Python 3.5.7, run:

$ pyenv install 3.5.7


Installing Buildout
--------------------

Check this package out of GitHub::

  $ git clone git@github.com:mromero107/farmandatsite_buildout.git

Cd into the ``farmandatsite_buildout`` directory.

Create a virtualenv::

  $ python3 -m venv .

Activate the virtualenv::

  $ source bin/activate

Upgrade pip::

  (farmandatsite_buildout)$ bin/pip install --upgrade pip

Install requirements::

  (farmandatsite_buildout)$ bin/pip install -r requirements.txt

Make sure that you have the latest version of setuptools for running the
buildout. Just do this from inside your virtualenv::

  (farmandatsite_buildout)$ bin/easy_install -U setuptools

After you've succesfully done the above, invoke the buildout via::

  (farmandatsite_buildout)$ bin/buildout -U

.. warning:: The ``-U`` flag above is *very important*.  It specifies
   to buildout that it should ignore the ``~/.buildout/default.cfg``
   file, which is often trampled upon by other software in ways that
   are incompatible with our usage of buildout.

When it's finished, an application named ``farmandatsite`` and its dependencies
(which include Substance D) should have been downloaded and compiled.  All
required Pyramid and Substance D software should also be installed within the
buildout environment.

You should then be able to run the following commands and visit the
running application at http://127.0.0.1:6541 in a browser.  You may
log in as ``admin`` with password ``admin`` to the management interface at
http://127.0.0.1:6541/manage::

  (farmandatsite_buildout)$ bin/supervisord
  (farmandatsite_buildout)$ bin/pserve etc/development.ini --reload

