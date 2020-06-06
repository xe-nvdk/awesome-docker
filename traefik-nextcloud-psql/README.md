## Traefik, Nextcloud y Postgress SQL
This example enable you to deploy Traefik as reverse proxy, Nextcloud and Postgress SQL.


Project structure:
```
.
├── docker-compose.yaml
└── README.md
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  traefik:
    container_name: traefik
    image: "traefik:v2.2"
    command:
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --providers.docker
      - --api
      - --api.dashboard=true
      - --log.level=DEBUG
      - --certificatesresolvers.leresolver.acme.httpchallenge=true
      - --certificatesresolvers.leresolver.acme.email=$email
      - --certificatesresolvers.leresolver.acme.storage=.acme/acme.json
      - --certificatesresolvers.leresolver.acme.httpchallenge.entrypoint=web
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
      - ".acme/acme.json:/acme.json"
    labels:
      - "traefik.http.routers.http-catchall.rule=hostregexp(`{host:.+}`)"
      - "traefik.http.routers.http-catchall.entrypoints=web"
      - "traefik.http.routers.http-catchall.middlewares=redirect-to-https"
      - "traefik.enable=true"
      - "traefik.http.routers.traefik.rule=Host(`$url_dashboard`)"
      - "traefik.http.routers.traefik.service=api@internal"
      - "traefik.http.routers.traefik.tls.certresolver=leresolver"
      - "traefik.http.routers.traefik.entrypoints=websecure"
      - "traefik.http.routers.traefik.middlewares=authtraefik"
      - "traefik.http.middlewares.authtraefik.basicauth.users=$httpd-auth"
      - "traefik.http.middlewares.redirect-to-https.redirectscheme.scheme=https"
    networks:
      - viernes

  nextcloud:
    container_name: nextcloud
    image: "nextcloud:latest"
    environment:
#      - NEXTCLOUD_ADMIN_USER=nacho
#      - NEXTCLOUD_ADMIN_PASSWORD=123456
      - POSTGRES_USER=nextcloud
      - POSTGRES_PASSWORD=123456
      - POSTGRES_HOST=postgres
      - POSTGRES_DB=nextcloud
      - NEXTCLOUD_TRUSTED_DOMAINS=$trusted_domain
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.nextcloud.rule=Host(`$url_nextcloud`)"
      - "traefik.http.routers.nextcloud.tls.certresolver=leresolver"
      - "traefik.http.routers.nextcloud.entrypoints=websecure"
    volumes:
      - ./nextcloud/html:/var/www/html
      - ./nextcloud/apps:/var/www/html/custom_apps
      - ./nextcloud/config:/var/www/html/config
      - ./nextcloud/data:/var/www/html/data
    depends_on:
      - "postgres"
    networks:
      - viernes

  postgres:
    container_name: postgres
    image: postgres:13
    environment:
      - POSTGRES_DB=nextcloud
      - POSTGRES_PASSWORD=123456
      - POSTGRES_USER=nextcloud
    volumes:
      - ./base:/var/lib/postgresql/data
    networks:
      - viernes

networks:
  viernes:
    external: true
```
This example is going to deploy Traefik, Nextcloud and Postgress SQL, you only need to define Nextcloud admin and password once the containers are created

Know Issues: As you can see, two environment variables are commented in the section of Nextcloud. This result in that way because if you specify credentials to make the install, NextCloud don't take the environment variable named NEXTCLOUD_TRUSTED_DOMAIN and that's required. So, it's recommended to define this variable and comment the user and password and define this credentials in the first time you visit your Nextcloud installation. The other issue is related to the creation of the admin users, take a long time to define everything is Postgress, after three minutes, everything works as expected.  

## Deploy this sample

```
$ docker network create viernes
$ docker compose up -d
Creating traefik ... done
Creating postgress  ... done
Creating nextcloud .... done
```

## Expected result

Check containers are running and the port mapping:

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                          NAMES
0f33ea260aee        nextcloud:latest    "/run.sh"                About a minute ago   Up 52 seconds       80/tcp                         nextcloud
09aa6d8b4dc5        traefik:v2.2        "/entrypoint.sh tele…"   About a minute ago   Up 51 seconds       0.0.0.0:80 0.0.0.0:443         traefik
0760db685a08        postgres:13         "/entrypoint.sh infl…"   About a minute ago   Up 51 seconds       5432/tcp                       postgress
```

Navigate to `https://$nextcloud_url` in your web browser to access the installed NextCloud.

Stop and remove the containers

```
$ docker-compose down
```

To delete all data, remove all named volumes by passing the `-v` arguments:

```
$ docker-compose down -v
```
