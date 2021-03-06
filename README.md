# newTYPO3Project

This project is based on [TYPO3-Documentation/site-introduction](https://github.com/TYPO3-Documentation/site-introduction).
It provides a whole development enviroment for new TYPO3 projects.

## Installation

To begin a new Project use this repository as development environment.
Please download and install [Docker](https://www.docker.com/products/docker-desktop) and [DDEV](https://github.com/drud/ddev/releases) first.

[LINUX / MacOS] Change permissions of ./var to 0777 (`chmod 0777 ./var/cache`) on host

* `ddev start`
* `ddev import-db --src=./data/db.sql`
* `ddev import-files --src=./assets`
* `ddev composer install`

## Frontend

* TYPO3: [https://introduction.ddev.site](https://introduction.ddev.site)
* Mail Hogg: [http://introduction.ddev.site:8025](http://introduction.ddev.site:8025)
* PHP My Admin: [http://introduction.ddev.site:8036](http://introduction.ddev.site:8036)

## Credentials Backend

* URL: [https://introduction.ddev.local/typo3](https://introduction.ddev.local/typo3)
* Username: `admin`
* Password: `password`

## Admin Tools

* URL: [https://introduction.ddev.site/typo3/install.php](https://introduction.ddev.site/typo3/install.php)
* Password: `password`

## Executing Commands

If you need to execute commands like `composer` or `bin/typo3` you need to run
these commands within the ddev containers. You can easily log into the web
container by executing the command `ddev ssh`. Its also possible to run commands
within the container without the need to log into it.

* Composer Install: `ddev composer install`
* Database Export: `ddev exec vendor/bin/typo3 ddev:exportdb`

## Using the typo3-console or the TYPO3 CLI

You can use the commands from the typo3-console and the TYPO3 CLI.

### typo3-console

        ddev typo3cms <command>

Example:

        ddev typo3cms cache:flush

        ddev typo3cms database:updateschema

        ddev typo3cms database:export

### TYPO3 CLI

        typo3 <command>

Example:

        ddev typo3 site:list

        ddev typo3 list

        ddev typo3 extension:list

        ddev typo3 ddev:exportdb

## Environment Variables

This setup is preconfigured to work with ddev. If you plan to use this setup
in a different context, please create a `.env` file and adapt the settings
to your system.

### .env.dist

    # Database Credentials
    TYPO3_DB_CONNECTIONS_DEFAULT_HOST = "db"
    TYPO3_DB_CONNECTIONS_DEFAULT_PORT = 3306
    TYPO3_DB_CONNECTIONS_DEFAULT_USER = "db"
    TYPO3_DB_CONNECTIONS_DEFAULT_PASS = "db"
    TYPO3_DB_CONNECTIONS_DEFAULT_NAME = "db"

    # Graphics
    TYPO3_GFX_PROCESSOR = "ImageMagick"
    TYPO3_GFX_PROCESSOR_PATH = "/usr/bin/"
    TYPO3_GFX_PROCESSOR_PATH_LZW = "/usr/bin/"

    # Mail
    TYPO3_MAIL_TRANSPORT = "smtp"
    TYPO3_MAIL_TRANSPORT_SMTP_SERVER = "localhost:1025"

    # System
    TYPO3_TRUSTED_HOST_PATTERN = "introduction.ddev.site"

    # Site
    SITE_INTRODUCTION_BASE = "http://introduction.ddev.site/"

## Saving changes

After you have worked in the backend, you should export the changes to keep them.
If you want to permanently add files, you should put them in the assets folder, which will be imported to the fileadmin folder.

* `ddev export-db --file=./data/db.sql`

## Testing

### Code maintenance

#### Execute acceptance tests

The ddev setup comes with a selenium-chrome container, codeception and some
acceptance tests ready to run.
[More information](https://docs.typo3.org/m/typo3/reference-coreapi/master/en-us/Testing/ProjectTesting.html)

* Run tests: `ddev composer test`

#### (TYPO3-)Rector

Rector can help you with maintenance your PHP code by analyse it for changes you should fix. Rector can even change your code for you. With the additional TYPO3-Rectors it can help you with keeping up with the changes in the TYPO3-System. [more >>]

Dry run:

    bin/typo3-rector process packages/customer_sitepackage --dry-run --config Tests/rector.php
Let's rector do the work for you:

    bin/typo3-rector process packages/customer_sitepackage  --config Tests/rector.php
You can config Rector in the Tests/rector.php file.

## Linting

### TypoScript Linting

    bin/typoscript-lint packages/ -c .tslint.yaml
