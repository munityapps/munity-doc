# How to start

## Get the code
```
git clone git@github.com:munityapps/munity.git
cd munity
```

## Prepare environment

In `.env` file set correct variables depending on your projet :

```
# URLS
HOMEPAGE_URL=localhost
APP_SUFFIX_URL=localhost
API_SUFFIX_URL=api.localhost

# API
API_DEBUG=True
GUNICORN_WORKER_NUMBER=5
API_LOG_LEVEL=debug
DB_HOST=db
DB_PORT=5432
DEFAULT_DB_NAME=munity

# DB
POSTGRES_PORT=5432
POSTGRES_USER=admin
POSTGRES_PASSWORD=****************
PGDATA=/var/lib/postgresql/data
```

## Start Munity
First your need to run the initialization script
```
./scripts/initialisation.sh
```

It will :

- Create munity and scheduler databases
- Migrate database to last version
- Prepare and start all service on current environement

Once initialization has been done one time, you just have to start project with :
```
docker-compose up -d
```

## For developer environment

### Bind local sources to docker

Code is copied in docker images at build, so if you change the code it will not update living code without rebuild your dockers.

To have a better developper experience you can simply create a copy from your local code to docker code directly.

To do so you have to create a new project file (that is git ignored) named `docker-compose.override.yaml` with following content :

```
version: '3.7'

services:
    api:
        volumes:
            - ./api:/usr/src/app
    celery_worker:
        volumes:
            - ./api:/usr/src/app
    celery_beat:
        volumes:
            - ./api:/usr/src/app
    app:
        volumes:
            - ./app:/var/www
```

### Use yarn start from docker

Futhermore, if you want to start React project in "server mode" to activate hot reload and other dev tools you can bind serve port in same file :

```
version: '3.7'

services:
    app:
        ports :
            - 3000:3000
```

This will permit to start app project with `docker-compose exec app yarn start` and go to `http://localhost:3000` for developement purpose.
**Important :** Since Munity detect backend url from frontend URL. You probabily have to adapt the backend URL in localstorage. Key is `munity_api_url`.

### Access database from the outside

By using the same approch you can bind Timescale database (postgresql in fact) to outside :

```
version: '3.7'

services:
    db:
        ports :
            - 65432:5432
```

You can connect to your database with client that you want on URL : `localhost:65432`.


### All in one

If you want all these modification you just have to create `docker-compose.override.yaml` file with following content :


```
version: '3.7'

services:
    api:
        volumes:
            - ./api:/usr/src/app
    celery_worker:
        volumes:
            - ./api:/usr/src/app
    celery_beat:
        volumes:
            - ./api:/usr/src/app
    app:
        volumes:
            - ./app:/var/www
        ports :
            - 3000:3000
    db:
        ports :
            - 65432:5432
```
