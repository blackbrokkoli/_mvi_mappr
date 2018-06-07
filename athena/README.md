# Running Athena

Athena is built using python 2.x. The recommened way is to use a virtual environment to run it.

All commands run in mappr-b/athena

## Machine Setup
==========
### Install Pip

https://pip.pypa.io/en/latest/installing.html

### Create virtual environment

1) Install virtualenv
$ sudo pip install virtualenv

2) Create the virtual environment. It will be stored in the .venv directory

$ virtualenv venv


## Running
==========
Make sure the venv directory exists in mappr-b/athena folder. If not, create it as directed above.

### Start virtualenv at mappr-b/athena/ (bash or its derivatives only. Use zsh :) )

$ . venv/bin/activate

Install libyaml

### Install project deps

$ pip install -r requirements.txt

### Running server

$ ./run_dev_mode.sh

Server autoloads when any source code changes.
