# Minecraft server platform managed by Docker

It provides a web application for managing the Minecraft server.
Also help with your website hosting.
When you associate each service with your domain, an SSL certificate is automatically applied.

A docker-compose aggregates any containers structured existing project.
The following projects are incorporated and containerized.
* [MineOS](https://github.com/hexparrot/mineos-node)
* [Nginx](https://github.com/nginx/nginx)
* [HTTPS-PORTAL](https://github.com/SteveLTN/https-portal)

## Requirements

* Docker >= 19
* Docker Compose >= 1.25

## Usage

### Environment Variables

Create `.env` in the project root.
Set the environment variables according to your own services.
```
# Configure certificate and reverse proxy settings managed by https-portal
DOMAINS=example.com -> http://www/, www.example.com -> http://www/, mineos.example.com -> http://mineos:8443

# MineOS Account
USERNAME=username
USERPASSWORD=password
```

* `DOMAINS` Configure certificate and reverse proxy settings managed by https-portal. [https-portal](https://github.com/SteveLTN/https-portal) provides a reverse proxy with Nginx and automates the issuance of certificates with Let's Encrypt. For details on the settings, please refer to the original document. 
* `USERNAME` MineOS's login username
* `USERPASSWORD` MineOS's login password

### Docker

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

Docker provides services locally to the following ports. 
Nginx inside https-portal listens for connections from external domains, and requests are reverse proxyed to various Docker services.

* 80, 443    : https-portal
* 8080       : any web site placed on `nginx/www`
* 8443       : mineos front-end
* 25565-25570: minecraft servers

Your website and MineOS frontend can be accessed over HTTPS by a reverse proxy.
The Minecraft server can be launched on port 25565 to 25570.
If you want to make it public, please allow ports 80, 443, 25565 to 25570 on your firewall.
