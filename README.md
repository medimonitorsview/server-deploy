# MediView - Medical Monitors Remote View Server Deploy

## Prerequisites

- 4 core machine with 16GB RAM
- ubuntu 16.04/18.04
- docker installed (https://docs.docker.com/engine/install/ubuntu/)
- git installed
- Either wget or curl


## First time install

```console
curl -fsSL https://raw.githubusercontent.com/medimonitorsview/server-deploy/master/scripts/deploy.sh | bash
cd server-deploy
docker-compose pull
# set env variables in .env file if needed
./scripts/setProperty.sh COVIEW_PORT 8888 .env
./scripts/setProperty.sh COVIEW_HOST 127.0.0.1 .env
./scripts/setProperty.sh COVIEW_ENABLED true .env
# start
docker-compose up
```

## To update

```console
cd server-deploy
git pull
docker-compose pull
```


### Obtain certificate or create a self signed one

(Currently unsed - using http and not https)

```
cd nginx/certs
openssl req -nodes -new -x509 -keyout server.key -out server.cert \
    -subj "/C=IL/ST=NRW/L=Haifa/O=Monitor/OU=DevOps/CN=monitor/emailAddress=dev@www.example.com"
```

