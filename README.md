# Docker-compose image for diacamma

this documentation mainly come from https://github.com/mgodlewski/dockerfiles/blob/master/diacamma/ 

http://www.diacamma.org/

## Building

to run Diacamma application, you need to 

- install Docker and Docker-compose >= 1.25
- dowload this repository
- build application localy
- modify configuration
- then run application

```
# download application code (docker-compose)
git cone https://github.com/mqu/docker-diacamma.git diacamma
cd diacamma

# build application localy in a docker container
docker-compose build

# edit docker-compose.yml variables
# then run application
docker-compose up -d

# open http://localhost:8100/
# on first run, you can choose any login/password ; that will create a new account for adminstrator
```

## Environment variables
Here are environment possible values

| var                     | possible values               | default                 | description                                        |
|:------------------------|:------------------------------|:------------------------|:---------------------------------------------------|
| DIACAMMA_TYPE           | syndic, asso                  | asso                    | run a diacamma-syndic or diacamma-asso instance    |
| DIACAMMA_ORGANISATION   | any string (no space allowed) | N/A                     | name for your Diacamma instance                    |
| DIACAMMA_DATABASE       | Refer to Database Section     | SQLite used if empty    | connection string to the database used by DIACAMMA; Only SQLite has be tested. |
| GUNICORN_EXTRA_CMD_ARGS | See [gunicorn settings](https://docs.gunicorn.org/en/stable/settings.html) | | add extra arguments to gunicorn                    |

## Data volumes exposed
| path in containers                | role                                 |
|:----------------------------------|:-------------------------------------|
| /backups                          | store backups and restorations files |
| /var/lucterios2/<organisation>    | store organisation setup files       |

It's strongly recommended to map those path onto your host.

## Building with sticked version

You can build an image with forced version of each component.

| Component          | default constaint | package                                              |
|:-------------------|:------------------|:-----------------------------------------------------|
| diacamma-asso      | empty (latest)    | https://pypi.org/project/diacamma-asso/#history      |
| diacamma-syndic    | empty (latest)    | https://pypi.org/project/diacamma-syndic/#history    |
| lucterios-standard | empty (latest)    | https://pypi.org/project/lucterios-standard/#history |

Constraint have to be compatible with pip install syntax (refer to https://www.python.org/dev/peps/pep-0440/#version-specifiers for more infos)



## Databases

- only SQLite database is supported and tested now
- building docker with mysql client fails 
- don't know how to include postgresql

## Backup and restore

Your diacamma instance must be running. Let's suppose that your container is named `diacamma` as in examples above.

```bash
docker exec diacamma backup
```
This will drop the backup file into your backups volume: `./backups/backup_20180901_20_26_55.lbkf`

In order to restore a backup file just run this command:

```bash
docker exec diacamma restore backup_20180901_20_26_55.lbkf
```

## FIXME

- support : open an issue here: https://github.com/mqu/docker-diacamma/issues

## TODO

- not having many time to support this application on docker.

