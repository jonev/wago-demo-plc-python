Master: [![Build Status](https://travis-ci.com/jonev/drinking-water-distribution-demo-ba-46-wago-ntnu-2020.svg?branch=master)](https://travis-ci.com/jonev/drinking-water-distribution-demo-ba-46-wago-ntnu-2020)

Develop: [![Build Status](https://travis-ci.com/jonev/drinking-water-distribution-demo-ba-46-wago-ntnu-2020.svg?branch=develop)](https://travis-ci.com/jonev/drinking-water-distribution-demo-ba-46-wago-ntnu-2020)

# README

## Prerequisites

Docker, Visual Studio Code (With remote development extension) and Git Bash

- Docker is used to run dependencies and development environment.
- Visual Studio Code is used to write code etc.
- Remote development extension is used to connect VS code to Docker development environment.
- Git bash is used to run build and deployment scripts.

## Development environment

- Open root folder in container with VS code.

## Run tests local

- Use vs code Test menu (recommended)
- Or from root:

```
python -m unittest discover test -p '*_test.py'
```

## Run tests as in Travis

1. `docker build -f Dockerfile-tests -t pythontestapp .`
2. `docker run -d --name app pythontestapp sleep infinity`
3. `docker ps -a`
4. `docker exec app python -m unittest discover test -p '*_test.py' -s test -v`
5. `docker rm -f app`

## Run current file, Leakdetection or Simulation program

- Use vs code run and debug menu, choose from the dropdown

## Build to PLC [(source)](https://www.docker.com/blog/multi-arch-images/)

### Prerequisites:

- To avoid long pull-time, re-use image tag
- Docker [hub account](https://hub.docker.com/)
- Docker settings -> Daemon -> Enable Experimental features
- Add builder:  
  `docker buildx create --name builder-for-plc`
- Configure to use it:
  `docker buildx use builder-for-plc`

### Build and push from root:

`docker buildx build -f dockerfile-name --platform linux/amd64,linux/arm64,linux/arm/v7 -t username/imagename:tag --push .`  
E.g:  
`docker buildx build -f Dockerfile-plc2-pressure --platform linux/amd64,linux/arm64,linux/arm/v7 -t jonev/ba-wago:v6 --push .`

- For the different applications, this build and push command is simplified with running the file: `./push.bat` or for each plc: `./plcX.bat`
- All images are build for `amd64`, `arm64` and `arm/v7` architectures.

### Common errors

- "No space left on device", run `docker system prune` and delete also images with `docker image prune -a`

## Run on PLC/HMI

### Prerequisites:

- Download [.ipk file](https://github.com/WAGO/docker-ipk/releases)
- Follow [this guide.](https://github.com/Wago-Norge/Docker-Support)

### Run

1. Add a `docker-compose.yaml` file to the PLC, containing the image
2. Connect to the PLC with SSH
3. Pull image with:  
   `docker-compose pull`
4. Run image with output to terminal connected:  
   `docker-compose up`

- Or run detached
  `docker-compose up -d`
- Or restart
  `docker-compose restart`
- Or restart only one container
  `docker-compose restart "service name"`

### Sources:

- [Docker on PFC200 2. Gen](https://github.com/Wago-Norge/Docker-Support)

## Tools

### SSH Client

Putty, or Termius (supports saving passwords and transfer files)

### Transfer files from Windows to Linux

[WinSCP](https://winscp.net/eng/download.php)

- Log inn and drag files between the two computers
