# Matomo

This repository allows for a local Docker-based Matomo instance running WMDE plugins. 

## Installation and configuration

### docker-compose environment

For setting up this repository you need to have Docker and [docker-compose](https://docs.docker.com/compose/) installed.

Start by copying the `.env.dist` file as `.env` in the project directory.
You may want to check the settings the `.env` file provides and adapt them to your needs as you see fit.  

Change the localhost port if it's already used bay a different
application.  

Chnage the frequency of the Matomo "cron" script when you're testing the
archiver functionality.


### Running the test environment

Start the database, web server and PHP with the command

    docker-compose up

## Matomo Configuration

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

## Installing custom plugins 

If you want to test the functionality, install the composer dependencies with a local composer binary:

    composer install

OR with the composer Docker image:

	docker run --rm -v $(pwd):/app -w /app composer install


## Using a local version of the plugin

If you're developing the plugin on your local machine and don't want to
push your changes to the repository and run `composer update` to test, you
can use the `docker-compose.yml` file to mount your local version in the
container:

In the `volumes` section of the `app` service, change the host part of the
volume that mounts to `/var/www/html/plugins/PLUGIN_NAME` to point to an 
absolute or relative path on your machine.

Example for a local version of the `CampaignVisitorsByTime` plugin, where
the directory `CampaignVisitorsByTime` is at the same level as
`fundraising-matomo` (this repository)

```
    volumes:
      - ./config:/var/www/html/config
      - ../CampaignVisitorsByTime:/var/www/html/plugins/CampaignVisitorsByTime
```

When changing volumes, you need to stop and start the containers with
`docker-compose stop` and `docker-compose start`.


## Enable custom plugins in Matomo

Custom plugins are disabled by default. Log in to your local Matomo, head
to the system configuration and enable them manually under System ->
Plugins. 

## Creating page impressions

The web server serves Matomo and a static page that you can use to create
page impressions for testing. Go to
[http://localhost:8090/static_test.html](http://localhost:8090/static_test.html) to see the test page.
Reload it to create page impressions. 

Remove the cookies starting with `_pk.id` and `_pk_session` to get a new
unique user identifier from Matomo.

**Make sure to disable your ad-blocker for your localhost as you may run into issues otherwise!**


