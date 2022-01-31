# Matomo

This repository allows for a local Docker-based Matomo instance running WMDE plugins. 

Make sure to disable your ad-blocker for your localhost as you may run into issues otherwise.

## Installation

For setting up this repository you need to have Docker and [docker-compose](https://docs.docker.com/compose/) installed.

Start by copying the `.env.dist` file as `.env` in the project directory.
You may want to check the settings the `.env` file provides and adapt them to your needs as you see fit. 
Especially the localhost port and the frequency of the Matomo cron may be relevant to you.

Next, install the Composer dependencies:

    composer install

Everything else is installed via docker-compose:

    docker-compose up

## Application Configuration

After running `docker-compose up`, Matomo is now accessible via:

    localhost:8090

Initially Matomo will be in a setup mode. When prompted for database credentials, configure it as follows:

    Database Server: db
    Login: matomo
    Password: omotan
    Database Name: matomo
	Database Prefix:

When prompted for a website URL, use (or whatever port you set in your .env file):

    http://localhost:8090

All other configurations are up to you.

After the application setup is complete, Matomo will show an error similar to this:

    Warning: You are now accessing Matomo from http://localhost:8090/index.php,
    but Matomo has been configured to run at this address: http://localhost/index.php.

This is due to the fact that the setup strips the port from the configuration file.
This can be fixed by manually editing the configuration file and adding the missing port.
The config in question can be edited as root via `config/config.ini.php` from within the project directory.

Simply replace the following line:

    trusted_hosts[] = "localhost"

With:

    trusted_hosts[] = "localhost:8090"

(You may have to update this value to the port which you set in your .env file).

## Enable custom plugins manually

Custom plugins are disabled by default. Head to the system configuration and enable them manually under System -> Plugins.
See `composer.json` for a list of installed plugins.

## Simple static testing.

A quick static test can be performed by directly accessing `static_test.html` in this directory. 
Some IDEs like PHPStorm may allow you to use their `run` functionality which helps with keeping the localhost domain when testing.
