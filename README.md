# oc-docker-web-dev

## Table of Contents
* [Description](#Description)
* [Getting Started]()
* [Documentation]()
* [Requirements]()

## Description
This project aims to offer a clean starting point for quickly building new websites with the Laravel powered OctoberCMS. Here is an overview of the technologies being leveraged:

* [Laravel](https://laravel.com)
* [OctoberCMS](octobercms/README.md)
* [Docker](https://www.docker.com/)
* [Laravel Mix](https://laravel.com/docs/5.8/mix)
* [Webpack](https://webpack.js.org/)
* [Node JS](https://nodejs.org/en/)
* [Bootstrap](https://getbootstrap.com/) 
* [JQuery](https://jquery.com/)

## Getting Started

### From Scratch
* In a terminal create a new folder and cd into it
* Create a new folder and initialize a git 
  * `git init`
* Clone the repository 
  * `git remote add  origin https://github.com/tfprado/oc-docker-web-dev.git my-project-name`
* Rename `docker-compose-exaple.yml` to `docker-compose.yml`
* Rename root folders `.env-example` to `.env` or create your own
  * [Changing docker configuration]() 
* Start up the docker containers for db/web/php
  * `docker-compose up -d --build`
* Exec into the docker php container to install composer dependencies
  * `docker exec -it docktober_php bash` 
  * `cd octobercms`
  * `composer install`

> You can now access the websites front-end at `localhost:8000` if using default configurations

### Setting up db/backend

> For accessing the backend OctoberCMS needs to connect to a database. You can set up a connection in the [database.php file](octobercms/config/database.php) or with a .env file
> 
#### database.php

* Set default connection to mysql

```php
'default' => 'mysql'
```

* Set the connection settings as follows

```php
'connections' => [
    'mysql' => [
        'driver'    => 'mysql',
        'host' => 'db', // Note db, same as container service name in docker-compose.yml file
        'port' => 3306, // Should match root .env file
        'database' => 'octobercms',
        'username' => 'root',  // Should match root .env file
        'password' => 'somepassword', // Should match root .env file
        'charset'   => 'utf8mb4',
        'collation' => 'utf8mb4_unicode_ci',
        'prefix'    => '',
    ]
]
```
### .Env

* After running `composer install` inside the docker container run `php artisan october:env` to change the configuration files accordingly. 
* OctoberCMS will create a .env file inside the `octobercms` folder and change the files inside `octobercms/config` accordingly. 

```
# OctoberCMS configuration example

APP_DEBUG=true
APP_URL=http://localhost
APP_KEY=base64:Se0FgML+1FU/kjN7siRKJ6DdC2LR6E2ZyPDnspefu9c=

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=octobercms
DB_USERNAME=root
DB_PASSWORD=somepassword
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
DB_USE_CONFIG_FOR_TESTING=false

CACHE_DRIVER=file

SESSION_DRIVER=file

QUEUE_DRIVER=sync

MAIL_DRIVER=smtp
MAIL_HOST=smtp.mailgun.org
MAIL_PORT=587
MAIL_ENCRYPTION=tls
MAIL_USERNAME=null
MAIL_PASSWORD=null

ROUTES_CACHE=false
ASSET_CACHE=false
LINK_POLICY=detect
ENABLE_CSRF=true
```

> Make sure the `DB_HOST` variable matches the service name of the container in the docker-compose.yml file.

```yml
version: '3'
services:
  db: # This is the name of your db host
    ...
```

### Final Steps
* After setting up the database configuration bash into the container if you haven't already 
  * `docker exec -it docktober_php bash`
* cd into `octobercms`
* run `php artisan october:up` to run the seeder and migrations
* You can now login to the admin panel at `localhost:8000/backend` with the default login `admin/admin`

## Documentation 

## Requirements

* [Docker]()