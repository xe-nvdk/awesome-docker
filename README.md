<p align="center">
  <img src="https://www.docker.com/sites/default/files/d8/styles/role_icon/public/2019-07/vertical-logo-monochromatic.png" width="128" height="128"/>
</p>

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/B0B34N5TU)

# awesome-docker
> A curated list of compose samples to run in Docke instances.

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/B0B34N5TU)

These samples provide a starting point for how to integrate different services using in Docker Compose. You're invited to contribute.

## Index of samples in this repository
If you're contributing, please, don't forget to modify this table and add info about your contribution. 

| Applications         | Description | Author |
|----------------------|-------------|:------:|
| [Calibre Web](calibre-web/) | This recipe enable you to run Calibre Web for storing, organizing and send eBooks to Kindle devices and others. | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |  
| [InfluxDB, Grafana and Telegraf](telegraf-influxdb-grafana/) | This recipe enable you to run three containers: InfluxDB 1.8, Telegraf and Grafana 7.0. | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |  
| [InfluxDB v2 Beta](influxdbv2) | This sample is a very simple docker-compose file to run InfluxDB v2 Beta. | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |
| [Traefik, Nextcloud and Postgress](traefik-nextcloud-psql) | This recipe enable you to run NextCloud and Postgress as backend with Traefik support https and dashboard. | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |
| [Traefik Pilot](traefik-pilot) | Sample recipe to run Traefik with Pilot feature. | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |
| [Traefik Portainer CE 2.0](traefik-portainer2.0) | Portainer CE 2.0 behind Traefik as Reverse Proxy | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |
| [Traefik Custom SSL](traefik-custom-ssl) | This recipe and configuration helps to deploy Traefik with custom ssl certificates. | [Ignacio Van Droogenbroeck](https://github.com/xe-nvdk) |


## How start with Docker Compose.
Make sure you have docker and docker-compose installed

## How to run a recipe
Just download one of this recipes and execute the following command:

```
$ docker-compose up -d
```

To stop and remove all containers deployed with an recipe, you only need to run:
```
$ docker-compose down
```

## Contribute

To submit a new template, see our [contributing guide] (In Construction)

## Support

If you need some help with any of this compose files, please, open a issue. See our [openning a issue guide] (In Construction)
