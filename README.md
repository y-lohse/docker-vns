# Docker for VNS project
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

This project is based on [ajardin repository](https://github.com/ajardin/docker-magento)

## Architecture
* `web`: [PHP 7.2 version](https://github.com/hochgenug/docker-vns/blob/master/web/Dockerfile) with Apache (web server).
* `mysql`: [mysql:latest](https://hub.docker.com/_/mysql/) image (database).
* `maildev`: [djfarrelly/maildev:latest](https://hub.docker.com/r/djfarrelly/maildev/) image (emails debugging).

## Additional Features
Since this environment is designed for a local usage, it comes with features helping the development workflow.

The `web` container has a mount point used to share source files.
By default, the `~/www/` directory is mounted from the host. It's possible to change this path by editing the `docker-compose.yml` file.

It's also possible to add custom virtual hosts: all `./web/vhosts/*.conf` files are copied in the Apache directory during the image build process.

And the `./web/custom.ini` file is used to customize the PHP configuration during the image build process. 

## Installation
This process assumes that [Docker Engine](https://www.docker.com/docker-engine) and [Docker Compose](https://docs.docker.com/compose/) are installed.
Otherwise, you should have a look to [Install Docker Engine](https://docs.docker.com/engine/installation/) before proceeding further.

### Clone the repository
```bash
$ git clone git@github.com:hochgenug/docker-vns.git vns
```

### Define the environment variables
```bash
$ cp docker-env.dist docker-env
$ nano docker-env
```

### Build the environment
```bash
$ docker-compose build
$ docker-compose up -d
```

### Check the containers
```bash
$ docker-compose ps
        Name                      Command               State                    Ports
--------------------------------------------------------------------------------------------------------
vns_web_1              docker-custom-entrypoint a ...   Up      80/tcp
vns_mysql_1            docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp
vns_maildev_1          bin/maildev --web 80 --smtp 25   Up      25/tcp, 0.0.0.0:1080->80/tcp
```
Note: You will see something slightly different if you do not clone the repository in a `vns` directory.
The container prefix depends on your directory name.

 ### Enter in the web container
 ```bash
 $ docker exec -it vns_web_1 bash
 ```
 (To get the name use the `docker-compose ps` command)
