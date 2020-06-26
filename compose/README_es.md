


















# API-Platform Híbrido - Docker Compose

El modelo de implantación **Híbrido** se recomienda para los clientes que tienen problemas de latencia.

A menudo se utiliza para las conexiones de **backends internos On-Premise**, por lo que no tiene sentido que los gateways se encuentren en la nube.

Esta documentación explica cómo implantar los módulos/servicios utilizados en el entorno híbrido.

## Requerimientos

* Docker 18.09 o superior
* Docker Compose 1.18 o superior
* Redis 4.0.11 o superior

## Docker

Instalar Docker CE (Community Edition) siguiendo las instrucciones de las páginas abajo de acuerdo con las distribuciones GNU/Linux

* CentOS:
https://docs.docker.com/install/linux/docker-ce/centos/

* Debian:
https://docs.docker.com/install/linux/docker-ce/debian/

* Ubuntu Server:
https://docs.docker.com/install/linux/docker-ce/ubuntu/
