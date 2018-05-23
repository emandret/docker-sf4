# Docker Symfony 4 environment

Edit the `.env` file according to your config :

    ###> app directory location ###
    APP_PATH=/var/www

    ###> app directory ###
    APP_DIR=app-dev

>**Notice :** `APP_DIR` is a bind mount relative to the `cwd` in the host and to the absolute `APP_PATH` in the container.

Alternatively, you can edit the Apache config at `./conf/httpd.conf`.

>**Warning :** the bind mount `./conf/httpd.conf:/usr/local/apache2/conf/httpd.conf` **must be** file-mounted (directly on `httpd.conf`) since the destination directory in the container is not empty.

## Development setup

Run :

    composer create-project symfony/skeleton app-dev
    docker-sync start
    docker-compose up -d

To stop sync :

    docker-sync stop && docker-sync clean

If you change the `app-dev` folder name you have to update the `src` value in the `docker-sync.yml` file :

    syncs:
      sf4-app-sync:
        src: './app-dev'
        # ...

## Production setup

Edit the `.env` file :

    ###> app directory location ###
    APP_PATH=/var/www

    ###> app directory ###
    APP_DIR=app-prod

Run :

    mv app-dev app-prod
    docker-compose -f docker-compose-prod.yml up -d

>An user-defined bridge network `docker-sf4` will be created by `docker-compose`.

## Troubleshooting

The Apache `DocumentRoot` is the `public` directory located in `app-dev`. This directory is not bind-mounted or a volume target so it will not be created when you run `docker-compose up`.

Normally, this directory is already present when you create a Symfony project with `composer`.

>Run `mkdir -p app-dev/public` to create it.
