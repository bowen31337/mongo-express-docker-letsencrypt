# MongoDB + Mongo Express + Docker Compose running with auto generate/renew Let's Encrypt Certificate

With this repo you will be able to set up self hosted [Mongo Express](https://github.com/mongo-express/mongo-express) as a container over SSL auto generated and auto renewed by a web proxy.

## PREREQUISITES

In order to use this compose file (docker-compose.yml) you must have:

- docker https://docs.docker.com/engine/installation/
- docker-compose https://docs.docker.com/compose/install/
- docker-compose-letsencrypt-nginx-proxy-companion https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion

## HOW TO USE 

1. Close this repository

```bash
$ git clone https://github.com/bowen31337/mongo-express-docker-letsencrypt.git
```

2. Make a copy of the `.env.example` and rename it to `.env`:

Update this file with your preferences.

```dotenv
#
# Container name for mongo
#
MONGO_CONTAINER_NAME=mongo

#
# Mongo Root User and Password
#

MONGO_ROOT_USERNAME=root
MONGO_ROOT_PASSWORD=root

#
# Init DataBase
#
MONGO_DATABASE_NAME=demo


#
# Container name for mongo express
#
MONGO_EXPRESS_CONTAINER_NAME=mongo-express

#
# Path where your Gitlab files will be located
#
MONGO_DATA_PATH=${PWD}/mongo/data/

#
# Your domain (or domains)
#
VIRTUAL_HOST=mongo.domain.com,www.mongo.domain.com

#
# Mongo express port number
#
VIRTUAL_PORT=8081


#
# Your domain (or domains) for SSL certificate
#
LETSENCRYPT_HOST=mongo.domain.com,www.mongo.domain.com

#
# Your email for Let's Encrypt register
#
LETSENCRYPT_EMAIL=your_email@domain.com


#
# Network name
# 
# Your container app must use a network connected to your webproxy 
# https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion
#
NETWORK=webproxy
```
3. Validate and view the docker-compose configuration before starting.

```bash
$ docker-compose config
```

4. Start the container.

During the build time, the environment variables are injected into the image.

```bash
$ docker-compose up -d
```

**Please keep in mind that when starting for the first time it may take a few moments (even a couple minutes) to get your Let's Encrypt certificates generated**

5. Basic Authentication Support

In order to be able to secure your virtual host with basic authentication, you must create a htpasswd file within ${NGINX_FILES_PATH}/htpasswd/${VIRTUAL_HOST} via:
```bash
sudo sh -c "echo -n '[username]:' >> ${NGINX_FILES_PATH}/htpasswd/${VIRTUAL_HOST}"
sudo sh -c "openssl passwd -apr1 >> ${NGINX_FILES_PATH}/htpasswd/${VIRTUAL_HOST}"
```
Please substitute the ${NGINX_FILES_PATH} with your path information, replace [username] with your username and ${VIRTUAL_HOST} with your host's domain. You will be prompted for a password. [More details](https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion#further-options)


