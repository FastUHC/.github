<br>
<br>

<div align="center">
  <a href="https://github.com/FastUHC">
    <img height="125" src="https://i.imgur.com/JN5FF7V.png">
  </a>
</div>

<br>
<br>

## About the project
[![Core/Api](https://wakatime.com/badge/user/74830337-5399-404a-b89c-ca7dc360e242/project/5a2da6a7-378f-4010-bd40-0d02cbe04030.svg)](https://wakatime.com/badge/user/74830337-5399-404a-b89c-ca7dc360e242/project/5a2da6a7-378f-4010-bd40-0d02cbe04030) [![fastuhc_backend](https://wakatime.com/badge/user/74830337-5399-404a-b89c-ca7dc360e242/project/417bb240-b565-4ea4-9a19-693ffc1f0317.svg)](https://wakatime.com/badge/user/74830337-5399-404a-b89c-ca7dc360e242/project/417bb240-b565-4ea4-9a19-693ffc1f0317) [![fastuhc_web](https://wakatime.com/badge/user/74830337-5399-404a-b89c-ca7dc360e242/project/5494a5a4-6ec5-4424-b960-8273dba0a499.svg)](https://wakatime.com/badge/user/74830337-5399-404a-b89c-ca7dc360e242/project/5494a5a4-6ec5-4424-b960-8273dba0a499)

***FastUHC*** is an infrastructure composed of a a **[Core/Api](https://github.com/FastUHC/FastUHC)**, **[Backend](https://github.com/FastUHC/fastuhc_backend)** and **[Web-Panel](https://github.com/FastUHC/fastuhc_web)** 

It's used to ***code*** an **UHC** plugin easly with *all* ***features*** of ***FastUHC's*** *system*.
With the **web panel** used to configure the **UHC** mini-game *coded* with the **API**.

## Installation
To install the entire ***FastUHC infrastructure***, you just have to make a **docker-compose** with some ***Traefik labels***.
You **need** to use ***Docker images*** given in others **repositories**. You just have to ***merge*** all **docker-compose** exemples in one like that.

> In this case, i'm using a **Traefik proxy**, you can **adapt it** with other *proxy*.

***Docker-Compose*** exemple:
```yml
version: "3"
services:
  backend:
    container_name: fastuhc_backend
    labels:
      - traefik.enable=true
      - traefik.http.routers.backend.rule=Host(`backend.localhost`)
      - traefik.http.services.backend.loadbalancer.server.port=8000
      - traefik.http.routers.backend.entrypoints=web
      - traefik.http.middlewares.cors.headers.accessControlAllowOriginList=*
      - traefik.http.middlewares.cors.headers.accessControlAllowMethods=GET,OPTIONS,PUT
      - traefik.http.middlewares.cors.headers.accessControlAllowHeaders=*
      - traefik.http.routers.backend.middlewares=cors
    networks:
      - internal
    restart: unless-stopped
    build:
      dockerfile: ./Dockerfile
      context: ./fastuhc_backend
    expose:
      - 8000

  web:
    container_name: fastuhc_web
    labels:
    - traefik.enable=true
    - traefik.http.routers.web.rule=Host(`web.localhost`)
    - traefik.http.services.web.loadbalancer.server.port=3000
    - traefik.http.routers.web.entrypoints=web
    networks:
      - internal
    restart: unless-stopped
    build:
      dockerfile: ./Dockerfile
      context: ./fastuhc_web
    environment:
      - NUXT_PUBLIC_BASE_URL=http://backend.localhost/
    expose:
      - 3000

  traefik:
    image: "traefik"
    container_name: "traefik"
    networks:
      - internal
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
networks:
  internal:
    driver: bridge
```

#### Informations
> To get info on **environnement variables** of services. Check on the target service's repository. For **Traefik** service, go on his [docs](https://doc.traefik.io/traefik/).

## Features
- Web panel to **configure** the mini-game for the ***host***
- Performances
- Easly to use
- Configuration files
- Docker

## Contributors
- [ForWarZz](https://github.com/ForWarZz)
