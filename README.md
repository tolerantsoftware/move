# Official configurations for the docker setup of TOLERANT Move
## The configurations for each released version can be found under tags


## Package Content

```
+-- .env                                 # a file containing variables for the compose file
+-- compose.yml                          # an example configuration for docker compose
+-- README.md
```

## Usage

### Starting a batch process
>**Note**
> the init service should only be executed once, to create the client database


#### First run:

```sh
docker compose up -d
```

Removing the container after it has exited

```sh
docker compose down
```

#### Later runs:
if you want to reuse an already existing client database
- comment out the init service in the `compose.yml`

if you want to create a new client database:
- remove the client database from the config volume
- adjust the entry point of the init service in the `compose.yml` to match the following pattern:
```yaml
   entrypoint: ["moveCreateClientDB.sh", "--customerno", "<customerno>", "--orderno", "<orderno>", "--name", "<name>", "--protocolPath", "/opt/tolerant/protocols", "--mkdir"]
```


#### Steps to use your own configuration and data for a batch process
- check out the section [Later runs](#later-runs)
- mount your configuration and data to the batch container
- adjust the command of the batch service in the `compose.yml` to match the following pattern:
```yaml
  command: "<configFilename>" "<projectId>" "<profileId>"
```

#### Starting with local user and group

To start with a different user please use the following instructions:

* create the following directories using the local user
    * move-config
    * move-data
    * move-logs
    * move-protocols
* use the fully qualified path of the above mentioned directories in the compose.yml
* comment in the **user** setting for the **init** and the **batch** service
* start the compose file

```sh
export UID=`id -u`; export GID=`id -g`; docker compose up -d
```

#### Stopping with local user and group

```sh
export UID=`id -u`; export GID=`id -g`; docker compose down
```