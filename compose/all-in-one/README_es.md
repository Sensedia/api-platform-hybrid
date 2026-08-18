<!-- TOC -->

- [Método de despliegue All-in-one](#método-de-despliegue-all-in-one)
  - [Módulos](#módulos)
  - [Requisitos](#requisitos)
- [Despliegue](#despliegue)
  - [Instalación](#instalación)
  - [Validación](#validación)
  - [Solución de problemas](#solución-de-problemas)

<!-- TOC END -->

# Método de despliegue All-in-one
> Última revisión: 2026-08-14

Este método de despliegue inicia todos los módulos *stateless* de la Plataforma en el mismo host.

> ¡¡¡ATENCIÓN!!! Este método no debe utilizarse en el entorno de producción. Este método se recomienda solo para demostraciones, pruebas o PoC (*Proof of concept*).

Si utiliza un Redis totalmente gestionado o ya cuenta con una instalación de Redis, este puede ser el modelo ideal.

## Módulos

En este método, instalar los módulos citados en [esta página](../README_es.md), en la sección **Módulos**.

## Requisitos

Acceder a [esta página](../README_es.md), en la sección **Requisitos**, para obtener información sobre cómo instalar las dependencias para el funcionamiento de los módulos.

# Despliegue

## Instalación
> ¡¡¡ATENCIÓN!!! Antes de iniciar la instalación, solicite las versiones y los datos necesarios para la instalación a través de un Ticket.

1 - Editar el archivo ``hybrid.env``, ubicado en este directorio, y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

2 - Generar un token de acceso del entorno híbrido en el API-Manager siguiendo las instrucciones de [esta página](../README_es.md) (sección: **Creación de Token**).

3 - Edite el archivo ``sensedia-all-in-one.yaml`` y cambie las versiones de los módulos que tienen los valores establecidos en **CHANGE_HERE**, de acuerdo con las instrucciones [en esta página](../README_es.md), en la sección **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros**.

4 - Usar los siguientes comandos para ejecutar ``docker-compose`` referenciando el archivo ``sensedia-all-in-one.yaml``.

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml up -d
```

## Validación

Verificar el estatus de los contenedores con el siguiente comando.

```bash
sudo docker-compose ps
```

Se mostrará una lista de servicios con el estatus correspondiente.

## Solución de problemas

Verificar los registros de cada servicio con el siguiente comando.

```bash
sudo docker-compose logs -f SERVICE_NAME
```

Sustituir el término ``SERVICE_NAME`` por el nombre del servicio cuyo registro se quiere ver.

Reiniciar el servicio con el siguiente comando.

```bash
sudo docker-compose restart SERVICE_NAME
```

Parar el servicio con el siguiente comando.

```bash
sudo docker-compose stop SERVICE_NAME
```

Iniciar el servicio una vez más con el siguiente comando.

```bash
sudo docker-compose start SERVICE_NAME
```

Si desea detener el servicio y eliminar todos los datos, volúmenes, imágenes y redes de los contenedores, use el siguiente comando (utilícelo sólo si realmente es necesario).

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml down
```

* Para validar su API, realizar una petición a la puerta de enlace híbrida; Accede a este enlace para la documentación de [validación](../validation/README_es.md).
