# Minecraft server platform managed by Docker
Utilizes Docker and Docker Compose to provide Minecraft server management and operation and secure web pages.

## Environment

Create `.env` in the project root.
Set the environment variables according to your own services.
```
# Configure certificate and reverse proxy settings managed by https-portal
DOMAINS=rokicraft.com -> http://www/, www.rokicraft.com -> http://www/, mineos.rokicraft.com -> http://mineos:8443

# MineOS Account
USERNAME=username
USERPASSWORD=password
```

## Docker

Create docker network `production`.
```
docker network create production
```

Create a directory for mounting if it doesn't exist.
```
mkdir certs minecraft/games nginx/www
```

Build and launch containers.
```
docker-compose build
docker-compose up -d
```

## Services

Docker provides services locally to the following ports. Nginx inside https-portal listens for connections from external domains, and requests are reverse proxyed to various Docker services.

* 80, 443    : https-portal
* 8080       : any web site placed on `nginx/www`
* 8443       : mineos front-end
* 25565-25570: minecraft servers
