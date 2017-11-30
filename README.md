# Alpine Magento CLI Image

![https://www.augustash.com](http://augustash.s3.amazonaws.com/logos/ash-inline-color-500.png)

**This base container is not currently aimed at public consumption. It exists as a starting point for August Ash containers.**

## Versions

- `1.0.2`, `latest` [(Dockerfile)](https://github.com/augustash/docker-alpine-magento-cli/blob/1.0.2/Dockerfile)
- `1.0.1` [(Dockerfile)](https://github.com/augustash/docker-alpine-magento-cli/blob/1.0.1/Dockerfile)
- `1.0.0` [(Dockerfile)](https://github.com/augustash/docker-alpine-magento-cli/blob/1.0.0/Dockerfile)

[See VERSIONS.md for image contents.](https://github.com/augustash/docker-alpine-magento-cli/blob/master/VERSIONS.md)

## Usage

This image provides the ability to run single, one-off commands as well as a long running PHP-FPM/Cron process. Run a single command like this:

```bash
docker run --rm \
    augustash/alpine-magento-cli magento cache:clean
```

```bash
docker run --rm \
    -v $(pwd)/conf/auth.json:/.composer/auth.json \
    augustash/alpine-magento-cli composer update
```

The image can also be used as part of a cluster for running `cron` and various Magento 2-specific utilities.

```bash
docker run -d -it \
    --name cli \
    --net <NETWORK NAME> \
    -v $(pwd)/conf/auth.json:/.composer/auth.json \
    -v $(pwd)/conf/crontab:/etc/crontabs/ash \
    -v $(pwd)/src:/src \
    augustash/alpine-magento-cli
```

### User/Group Identifiers

To help avoid nasty permissions errors, the container allows you to specify your own `PUID` and `PGID`. This can be a user you've created or even root (not recommended).

### Environment Variables

The following variables can be set and will change how the container behaves. You can use the `-e` flag, an environment file, or your Docker Compose file to set your preferred values. The default values are shown:

- `PUID`=501
- `PGID`=1000
- `DEBUG`=false
- `PHP_MEMORY_LIMIT`=2G
- `MAGENTO_PATH`=/src
- `MAGENTO_CODEBASE`=cc
- `MAGENTO_USE_SAMPLE_DATA`=false
- `MAGENTO_INSTALL_DB`=false
- `MAGENTO_BASE_URL`=http://magento.dev/
- `MAGENTO_DB_HOST`=mysql
- `MAGENTO_DB_NAME`=magento2
- `MAGENTO_DB_USER`=user
- `MAGENTO_DB_PASSWORD`=password
- `MAGENTO_ADMIN_FIRSTNAME`=August
- `MAGENTO_ADMIN_LASTNAME`=Ash
- `MAGENTO_ADMIN_EMAIL`=user@example.com
- `MAGENTO_ADMIN_USER`=augustash
- `MAGENTO_ADMIN_PASSWORD`=password
- `MAGENTO_BACKEND_FRONTNAME`=random-admin-url
- `MAGENTO_LANGUAGE`=en_US
- `MAGENTO_CURRENCY`=USD
- `MAGENTO_TIMEZONE`=America/Chicago
- `MAGENTO_REWRITES`=1

## Utilities

The following utilties are installed and available by default within this image. All scripts proxy to their official counterpart but run by the `ash` user instead of `root`.

### `composer`

Runs [Composer](https://getcomposer.org/), the defacto dependency manager for PHP.

### `grunt`

Runs [Grunt](https://gruntjs.com/), the JavaScript task runner.

### `gulp`

Runs [Gulp](https://gulpjs.com/), which is an alternate JavaScript task runner.

### `installer`

Custom script to install and configure a new Magento 2 instance. Requires the `MAGENTO_` environment variables defined above.

### `magento`

A proxy to the `bin/magento` tool that comes with Magento 2.

### `n98`

Runs [n98-magerun2](http://magerun.net/), the swiss army knife for Magento developers, sysadmins and devops.

## Sample Magento 2 Commands

When this image is used as part of a Magento 2 cluster, the following commands can be useful:

```bash
docker-compose run --rm magento-cli magento deploy:mode:set developer
docker-compose run --rm magento-cli magento dev:urn-catalog:generate .idea/misc.xml
docker-compose run --rm magento-cli magento setup:upgrade
docker-compose run --rm magento-cli magento setup:static-content:deploy
docker-compose run --rm magento-cli magento cache:flush
```

## Inspiration

- [fballiano](https://github.com/fballiano/)
- [joshporter](https://github.com/joshporter)
- [meanbee](https://github.com/meanbee/)
