mappr - a network analysis and visualization platform.

## How to Run

There are 2 ways to run the system

- Via Docker. This is a good way to go if you want to just use the application
- Running locally. This is the preffered way to develop the system. However, it can, and probably should be dockerised as well.

## How to run via docker

Make sure you have installed docker.

### Pre-run setup(Do this if you're running for first time, or if there are any changes to the code)

1) `docker-compose stop` - stops the running services, if any.
2) `docker-compose build` - build the relevant images. Required if any of the source file changes. It takes awhile so be patient

### Run openmappr
* `docker-compose up` - start the various services and get the project running. Once the project is running, open http://localhost:8080 and login as `user@mappr.io` with password `woot`.

Note:- This is actually a server running, so don't kill the terminal unless you want to exit.

### Exit openmappr
* Press 'Ctrl-C' at the terminal to exit.

## Running it locally

To run it locally, we need 6 things

- Mongo running locally on port 27017. It is the default mongo port.
- Beanstalkd - http://kr.github.io/beanstalkd/
- python 2.x for running athena. Refer to the athena [Readme.md](athena/README.md) for details.
- Node 6 or greater for running the server. Refer to Node Env setup guide section.
- Sass. Refer to Sass install section.
- Elasticsearch 2.4. Optional component needed for search.

there are 2 scripts for running dev version of webapp.

- `run_local_mode.sh` - the most common. run the server in local mode
- `run_test_mode.sh` - runs the server in testing mode. mostly for testing apis and other things.

### Steps

These are the steps needed to get the full system running locally.
* have mongodb running at 27017 using ` mongod --config /usr/local/etc/mongod.conf & ` (if installed via brew)
* Have beanstalkd running. `beanstalkd &` (if installed via brew)
* Run elasticsearch. `elasticsearch &` (if installed via brew)
* Run athena via `./run_dev_mode.sh`. Ensure virtual environment was created as directed by [Readme.md](athena/README.md)
* Build webapp
    * Ensure all packages are installed by doing the steps listed in the Setting up the node environment section.
    * `grunt` to build webapp. Then do `grunt watch` to watch for source code changes
* `./run_local_mode.sh` - for running the webapp
* point your browser to localhost:8080 and login as `user@mappr.io` with password `woot`.

## Sever-side Organization
Code is divided into top level modules, each with routes, controllers, models and services. (if needed)

### Top Level Modules

* auth            - user authentication, includes passport config.
* common          - common elements like auth middlewares, permission middleware.
* commonOps       - commonOps module. has its own Readme.md file.
* config          - all common / dev / prod / testing config is made available as a module.
* data_api        - module for data api. makes networks available through the API
* datasys         - central data wrangling module. upload data, CRUD on dataset and networks.
* etl             - ETL pipeline. houses the ETL engine which runs scripts on spark cluster
* libs            - external libs outside of npm, mostly copy pasted code
* migrators       - migration code. Used when schema is changed, or need to run system wide validations
* misc_controllers- small controllers which interface with a service. like job / elasticsearch / svg / maintenance controllers
* models          - (depreciated -> contains schema for the old db. Used by survey and hence not removed)
* orgs            - module for organization management.
* player          - module for player managment. also contains a top_router for player specific routes
* project         - project module. cloning, deleting and so on.
* recipe          - Recipe Engine module. each individual stage has its own file.
* schemas         - All schemas are here. This is because I feel they might be shared across multiple projects
* scriptStore     - contains script runner to run generic scripts on data. depreciated
* services        - common Services needed by all modules
* snapshot        - snapshot module.
* survey          - survey module
* user            - user management module
* utils           - common utility functions. Also contains parsing code.

### Top Level files
- main_server.js - the server initialization code.
- main_router.js - Each modules furnishes its own router. Which is combined together in the this file.
- admin_router.js - similar to main_routers, for adminstrator account.
- misc_routes.js - bunch of routes, connecting misc_controllers
- routes.js - all routes which have not been ported over.
- ../server.js - entry point. Ensures node 5 is running.



# Setting up Node environment

The best way to setup Node is to use a node version manager. One way to do it is to use nvm.


* install nvm from https://github.com/creationix/nvm. follow the guide. Do not use brew to install it.

also, when editing ~/.bashrc, ~/.profile, ~/.zprofile, or ~/.zshrc, use below:
```
export NVM_DIR="$HOME/.nvm"
export NVM_SYMLINK_CURRENT="true" # for editors to work properly
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```
also, if using zsh, update ~/.zshenv or ~/.zprofile instead of ~/.zshrc

* install node
```
nvm install 6.0
nvm use 6.0
nvm alias default node
```

* install basic packages
```
npm install -g yo bower grunt-cli
```

* install packages
```
npm install
bower install
```
* enter `grunt` to build the webapp. if developing, use `grunt watch` after that to watch for changes.

* Run `./run_local_mode.sh` to run the webapp.

# Sass

Sass and Compass need to be installed in order for sass to compile to css.

* make sure ruby is installed first (by default on macs)

* install sass
*
```
gem install sass
```

[Sass install](http://sass-lang.com/install)

* install Compass

```
gem install compass
```
[Compass install](http://compass-style.org/install/)
