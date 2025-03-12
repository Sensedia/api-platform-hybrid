<!-- TOC -->

- [API-Platform Híbrido - Docker Compose Modules](#api-platform-híbrido---docker-compose-modules)
  - [Requisitos](#requisitos)
- [Despliegue](#despliegue)
  - [Instalación](#instalación)
    - [Logstash](#logstash)
    - [Agent-Authorization](#agent-authorization)
    - [Agent-Gateway](#agent-gateway)
    - [API-Authorization](#api-authorization)
    - [API-Gateway](#api-gateway)
  - [Validación](#validación)
  - [Solución de problemas](#solución-de-problemas)

<!-- TOC -->

# API-Platform Híbrido - Docker Compose Modules

En este método, todos los módulos se pueden instanciar por separado.

## Requisitos

Acceder a la sección **Requisitos** en [esta página](../README.md) para obtener información sobre como instalar las dependencias para el funcionamiento de los módulos.

# Despliegue

## Instalación

### Logstash

* Editar el archivo ``logstash-federated/logstash.env`` y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

* Editar el archivo ``logstash-federated/logstash-federated.yaml`` y cambiar la version del módulo según las instrucciones de la sección **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros** de [esta página](../README.md).

* Ejecutar el servicio usando ``docker-compose`` con el siguiente comando.

```bash
sudo docker-compose -f logstash-federated/logstash-federated.yaml up -d
```

### Agent-Authorization

* Editar el archivo ``agent-authorization/agent-authorization.env`` y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

* Crear un token de acceso del entorno híbrido en el API-Manager según  las instrucciones de la sección **Creación de Token** de [esta página](../README.md).

* Editar el archivo ``agent-authorization/agent-authorization.yaml`` y cambiar la version del módulo según las instrucciones de la sección **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros** de [esta página](../README.md).

* Ejecutar el servicio usando ``docker-compose`` con el siguiente comando.

```bash
sudo docker-compose -f agent-authorization.yaml up -d
```

### Agent-Gateway

* Editar el archivo ``agent-gateway/agent-gateway.env`` y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

* Crear un token de acceso del entorno híbrido en el API-Manager según  las instrucciones de la sección **Creación de Token** de [esta página](../README.md).

* Editar el archivo ``agent-gateway/agent-gateway.yaml`` y cambiar la version del módulo según las instrucciones de la sección **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros** de [esta página](../README.md).

* Ejecutar el servicio usando ``docker-compose`` con el siguiente comando.

```bash
sudo docker-compose -d -f agent-gateway/agent-gateway.yaml up
```

### API-Authorization

* Editar el archivo ``api-authorization/api-authorization.env`` y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

* Editar el archivo ``api-authorization/api-authorization.yaml`` y cambiar la version del módulo según las instrucciones de la sección **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros** de [esta página](../README.md).

* Ejecutar el servicio usando ``docker-compose`` con el siguiente comando.

```bash
sudo docker-compose -f api-authorization/api-authorization.yaml up -d
```

### API-Gateway

* Editar el archivo ``api-gateway/api-gateway.env`` y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno.

* Editar el archivo ``api-gateway/api-gateway.yaml`` y cambiar la version del módulo según las instrucciones de la sección **Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros** de [esta página](../README.md).

* Ejecutar el servicio usando ``docker-compose`` con el siguiente comando.

```bash
sudo docker-compose -f api-gateway/api-gateway.yaml up -d
```

## Validación

Verificar el estatus de los contenedores con el siguiente comando.

```bash
sudo docker-compose -f NOME_DIR/NOME_COMPOSE_FILE ps
```

Sustituir el término ``NOME_DIR`` por el directorio del servicio.

Sustituir el término ``NOME_COMPOSE_FILE`` por el archivo del servicio.

Se mostrará una lista de servicios con el estatus correspondiente.

## Solución de problemas

Verificar los registros de cada servicio con los siguientes comandos.

```bash
cd NOME_DIR
sudo docker-compose logs -f SERVICE_NAME
```

Sustituir el término ``NOME_DIR`` por el directorio del servicio.

Sustituir el término ``SERVICE_NAME`` por el nombre del servicio cuyo registro se quiere ver.

Reiniciar el servicio con los siguientes comandos.

```bash
cd NOME_DIR
sudo docker-compose restart SERVICE_NAME
```

Parar el servicio con los siguientes comandos.

```bash
cd NOME_DIR
sudo docker-compose stop SERVICE_NAME
```

Iniciar el servicio una vez más con los siguientes comandos.

```bash
cd NOME_DIR
sudo docker-compose start SERVICE_NAME
```

Se quiere parar el servicio y quitar todos los dados, volúmenes, imágenes, red de contenedores, usar el siguiente comando (usar sólo si realmente necesario).

```bash
cd NOME_DIR
sudo docker-compose -f SERVICE_NAME.yaml down
```

* Para validar su API, realizar una petición a la puerta de enlace híbrida; Accede a este enlace para la documentación de [validación](../validation/README_pt.md).
