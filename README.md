# Docker compose for Magento2 installed in an unattended way

## Docker Environment

To use the docker environment clone this repo inside a folder structure like this:

```
some_directory <- Name is not important
  |- src <- An empty folder to be used in the automatic installation process
```

And copy this repo's `docker-compose.yml` & `build.sh` & `.env` files to the root of the base directory (`some_directory` in the example) so you'll have:

```
some_directory <- Name is not important
  |- src <- An empty folder to be used in the automatic installation process
  |- docker-compose.yml
  |- build.sh
  |- .env
```

Edit the copy of `.env` and set your tokens in the `COMPOSER_AUTH` environment variable. You can also set the version of Magento you wish to install.

Then run the environment by executing:

```
$ docker-compose up
```

from the base directory where the copy of `docker-compose.yml` is located.
The installation process will take some minutes to be finished. You can follow the status logging with:

```docker logs setup -f```

Finally, Magento 2 with the module installed will be running at `http://localhost:80`.

The admin panel will be available at `http://localhost:80/admin`. Admin credentials are easy:

```
User: admin
Pass: admin123
```

To install sample data, with the containers running:

```
$ docker exec -u root -it web bash
root@...:~# cd /app
root@...:/app# gosu application php -d memory_limit=-1 ./bin/magento sampledata:deploy
root@...:/app# gosu application php -d memory_limit=-1 ./bin/magento setup:upgrade
```

## Exposing the server to internet

To expose our local server to internet we can use, for example; the utility Ngrok: https://dashboard.ngrok.com/get-started/setup

Once we have the external ip created (and before running the docker-compose up) simply edit the .env file and set the MAGENTO_BASE_URL=ip (for example: MAGENTO_BASE_URL=7dd5-80-26-218-151.ngrok.io)

So, when the installation process finished, instead of accessing to http://localhost:80 we will use: http://ip:80 (for example: https://7dd5-80-26-218-151.ngrok.io).

## Xdebug ready to use

If you wish to debug your new Magento installation, just simply set the correct values in `.env` and configure your IDE attending to the remote PHP docker container `web`. You should also bind your local source path: `./src` to the docker one: `/app`

## Last notes

Please, take care when you change in `.env` the MAGENTO_VERSION parameter since you'll have to change probably the PHP_VERSION & COMPOSER_VERSION ones in order to maintain the compatibility. For example, if you wish the Magento 2.4.3 version you should have:

```
PHP_VERSION=7.4
COMPOSER_VERSION=2.0.14
MAGENTO_EDITION=community
MAGENTO_VERSION=2.4.3
```
but if you want to test, let's say, the 2.3.1 version you should have something like this:

```
PHP_VERSION=7.2
COMPOSER_VERSION=1.4.3
MAGENTO_EDITION=community
MAGENTO_VERSION=2.3.1
```
And please, don't forget to copy in `.env` your Magento repository tokens filling the parameters:
```
COMPOSER_AUTH_USERNAME=
COMPOSER_AUTH_PASSWORD=
```
