# Dockfony

Docker, but for Symfony

## Forked

This repository was forked from `shipping-docker/vessel` and their website can be found here [https://vessel.shippingdocker.com](https://vessel.shippingdocker.com).

They did fantastic work and this repository is simply a conversion to handle Symfony instead. All credit really goes to them.

## Install

Dockfony is just a small set of files that sets up a local Docker-based dev environment per project. There is nothing to install globally, except Docker itself!

This is all there is to using it:

```bash
composer req dockfony

bash dockfony init

./dockfony start
```

Head to `http://localhost` in your browser and see your Symfony site!

## Multiple Environments

Dockfony attempts to bind to port 80 and 3306 on your machine, so you can simply go to `http://localhost` in your browser.

However, if you run more than one instance of Dockfony, you'll get an error when starting it; Each port can only be used once. To get around this, use a different port per project by setting the `APP_PORT` and `MYSQL_PORT` environment variables in one of two ways:

Within the `.env` file:

```
APP_PORT=8080
MYSQL_PORT=33060
```

Or when starting Dockfony:

```bash
APP_PORT=8080 MYSQL_PORT=33060 ./dockfony start
```

Then you can view your project at `http://localhost:8080` and access your database locally from port `33060`;

## Common Commands

Here's a list of built-in helpers you can use. Any command not defined in the `dockfony` script will default to being passed to the `docker-compose` command. If not command is used, it will run `docker-compose ps` to list the running containers for this environment.

### Show Dockfony Version or Help

```bash
# shows Dockfony current version
./dockfony --version # or [ -v | version ]

# shows Dockfony help
./dockfony --help # or [ -H | help ]
```

### Starting and Stopping Dockfony

```bash
# Start the environment
./dockfony start

## This is equivalent to
./dockfony up -d

# Stop the environment
./dockfony stop

## This is equivalent to
./dockfony down
```

### Development

```bash
# Use composer
./dockfony composer <cmd>
./dockfony comp <cmd> # "comp" is a shortcut to "composer"

# Run phpunit tests
./dockfony test

## Example: You can pass anything you would to phpunit to this as well
./dockfony test --filter=some.phpunit.filter
./dockfony test tests/Unit/SpecificTest.php


# Run npm
./dockfony npm <cmd>

## Example: install deps
./dockfony npm install

# Run yarn

./dockfony yarn <cmd>

## Example: install deps
./dockfony yarn install

# Run gulp
./dockfony gulp <cmd>
```

### Docker Commands

As mentioned, anything not recognized as a built-in command will be used as an argument for the `docker-compose` command. Here's a few handy tricks:

```bash
# Both will list currently running containers and their status
./dockfony
./dockfony ps

# Check log output of a container service
./dockfony logs # all container logs
./dockfony logs app # nginx | php logs
./dockfony logs mysql # mysql logs
./dockfony logs redis # redis logs

## Tail the logs to see output as it's generated
./dockfony logs -f # all logs
./dockfony logs -f app # nginx | php logs

## Tail Symfony Logs
./dockfony exec app tail -f /var/www/html/var/log/symfony.log

# Start a bash shell inside of a container
# This is just like SSH'ing into a server
# Note that changes to a container made this way will **NOT**
#   survive through stopping and starting the Dockfony environment
#   To install software or change server configuration, you'll need to
#     edit the Dockerfile and run: ./dockfony build
./dockfony exec app bash

# Example: mysqldump database "homestead" to local file system
#          We must add the password in the command line this way
#          This creates files "homestead.sql" on your local file system, not
#          inside of the container
# @link https://serversforhackers.com/c/mysql-in-dev-docker
./dockfony exec mysql mysqldump -u root -psecret symfony > symfony.sql
```


## What's included?

The aim of this project is simplicity. It includes:

* PHP 7.3
* MySQL 5.7
* Redis ([latest](https://hub.docker.com/_/redis/))
* NodeJS ([latest](https://hub.docker.com/_/node/)), with Yarn & Gulp

## How does this work?

If you're unfamiliar with Docker, try out this [Docker in Development](https://serversforhackers.com/s/docker-in-development) course, which explains important topics in how this is put together.

If you want to see how this workflow was developed, check out [Shipping Docker](https://serversforhackers.com/shipping-docker) and signup for the free course module which explains building this Docker workflow.

## Supported Systems

Dockfony requires Docker, and currently only works on Windows, Mac and Linux.

> Windows requires running Hyper-V.  Using Git Bash (MINGW64) and WSL are supported.  Native
  Windows is still under development.

| Mac                                                                      |                                              Linux                                              |                                     Windows                                      |
| ------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------: |
| Install Docker on [Mac](https://docs.docker.com/docker-for-mac/install/) | Install Docker on [Debian](https://docs.docker.com/engine/installation/linux/docker-ce/debian/) | Install Docker on [Windows](https://docs.docker.com/docker-for-windows/install/) |
|                                                                          | Install Docker on [Ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/) |                                                                                  |
|                                                                          | Install Docker on [CentOS](https://docs.docker.com/engine/installation/linux/docker-ce/centos/) |                                                                                  |
