# MediView - Medical Monitors Remote View Server Deploy

This repo contains the deployment code for MediView medical monitors remote recording system.

The system is composed from a central server, and two android applications - [recording app](https://github.com/medimonitorsview/recorder-app) and [setup app](https://github.com/medimonitorsview/setup-app).
The central server has three components - [backend](https://github.com/medimonitorsview/backend), [image processing](https://github.com/medimonitorsview/monitor-cv) and [frontend](https://github.com/medimonitorsview/frontend).  

This repo contains the installation scripts.

## System overview

This system uses android phones positioned in front of medical monitors (and other medical devices) screens, to record and collect their data to a central server, and display the data in a control station ui.

Each monitor has a phone positioned in front of it, and takes photos of the screen. It then processes the image and performs OCR (Optical Character Recognition), and send the measurement values the the backend server. The server continues to clean up the data from all the devices, and enables accessing the data on a web UI.

To add a monitor to the system one should print QR code form the web UI and stick it to the medical monitor. The scan it with the recording app and start recording the screen (see recording app for detailed instructions). Next scan the QR on the device with the setup app (The QR appears also on the web UI), and choose the locations and types of the measurement you want to record. 
Pay attention to carefully position the device and select the measurement. After (or before) you entered measurements information enter patient information in the settings app, and the device data will start to appear on the web ui.


## Installation

After the installation is complete the system will be available as a web server on any server IP.

### Prerequisites

- 4 core machine with 16GB RAM
- Ubuntu 16.04/18.04
- Docker installed (https://docs.docker.com/engine/install/ubuntu/)
- Git installed
- Either wget or curl


### First Time Install

```bash
sudo apt-get update
sudo apt-get install -y git docker.io curl
curl -fsSL https://raw.githubusercontent.com/medimonitorsview/server-deploy/master/scripts/deploy.sh | sudo bash
git clone https://github.com/medimonitorsview/server-deploy
cd server-deploy
docker-compose pull
# set env variables in .env file if needed
./scripts/setProperty.sh COVIEW_PORT 8888 .env
./scripts/setProperty.sh COVIEW_HOST 127.0.0.1 .env
./scripts/setProperty.sh COVIEW_ENABLED true .env
# start
docker-compose up
```

Check the `docker-compose.yml` file for additional configuration variables.


### Update

```bash
cd server-deploy
git pull
docker-compose pull
```

You can also change env variables as in first time install.

## Troubleshooting:

- In case of certificate validation problems, please install the correct certificates on the server, you can try pulling them from the actual certs used,
but *please* inspect them first:

```console
openssl s_client -showcerts -connect www.google.com:443 < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ca.crt
cp ca.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
```

- Obtain certificate or create a self signed one, to use https:

```console
cd nginx/certs
openssl req -nodes -new -x509 -keyout server.key -out server.cert \
    -subj "/C=IL/ST=NRW/L=Haifa/O=Monitor/OU=DevOps/CN=monitor/emailAddress=dev@www.example.com"
```

You will need to add this cert to all user devices.


