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

Este método de despliegue inicia todos los módulos *stateless* de la Plataforma en el mismo host.

Si usa Redis administrado o tiene una instalación de Redis, este modelo puede ser el mejor.

## Módulos

En este método, instalar los módulos citados en [esta página](../README_es.md), en la sección **Módulos**.

## Requisitos

Acceder a [esta página](../README_es.md), en la sección ``Requisitos``, para obtener información sobre cómo instalar las dependencias para el funcionamiento de los módulos.

# Despliegue

## Instalación

1 - Editar el archivo ``hybrid.env``, ubicado en este directorio, y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

2 - Generar um token de acesso del entorno híbrido en lo API-Manager siguiendo las instrucciones de [esta página](../README_es.md) (sección: **Creación de Token**).

3 - Editar el archivo ``sensedia-all-in-one.yaml`` y cambiar las versiones de los módulos según las instrucciones de [esta página](../README_es.md) (sección: **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros**).

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

Se quiere parar el servicio y quitar todos los dados, volúmenes, imágenes, red de contenedores, usar el siguiente comando (usar sólo si realmente necesario).

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml down
```
