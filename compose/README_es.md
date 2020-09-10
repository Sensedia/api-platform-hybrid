<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [API-Platform Híbrido - Docker Compose](#api-platform-híbrido---docker-compose)
  - [Requerimientos](#requerimientos)
  - [Docker](#docker)
    - [Docker-Compose](#docker-compose)
    - [Selinux en CentOS/Red Hat](#selinux-en-centosred-hat)
    - [Redis](#redis)
  - [Métodos de Despliegue](#métodos-de-despliegue)
    - [Módulos](#módulos)
    - [Recursos Recomendados](#recursos-recomendados)
    - [Ejemplos de Deployment](#ejemplos-de-deployment)
    - [Despliegue de Servicios](#despliegue-de-servicios)
  - [Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros](#cambio-de-versión-de-los-módulos-puerto-de-red-y-otros-parámetros)
    - [Creación de Token](#creación-de-token)
<!-- TOC END -->




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

Iniciar el servicio Docker, configurar el Docker para que se inicie en el arranque del sistema operativo y añadir su usuario al grupo Docker.

```bash
# Iniciar el servicio Docker
sudo systemctl start docker

# Configurar el Docker para que se inicie en el arranque del S.O
sudo systemctl enable docker

# Añadir su usuario al grupo Docker
sudo usermod -aG docker $USER
sudo setfacl -m user:$USER:rw /var/run/docker.sock
```

Fuente: https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot

### Docker-Compose

Instalar Docker Compose con los siguientes comandos.

```bash
COMPOSE_VERSION=1.25.5

sudo curl -L "https://github.com/docker/compose/releases/download/$COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

/usr/local/bin/docker-compose version
```

Más informaciones pueden ser obtidas en la documentación oficial: https://docs.docker.com/compose/install/.

### Selinux en CentOS/Red Hat

Selinux puede interferir con los permisos de archivos y procesos. Para evitar problemas, desactívelo con los siguientes comandos.

```bash
sudo setenforce 0
```

Editar el archivo ``/etc/sysconfig/selinux`` y cambiar el siguiente parámetro.

Antes:

```
SELINUX=enforcing
```

Después:

```
SELINUX=disabled
```

Reiniciar el host para aplicar la alteración.

Fuente: https://www.hostinger.com.br/tutoriais/desabilitar-selinux-centos-7/

### Redis

Hay varias opciones para ejecutar Redis.

|Método de despliegue|Entorno|Detalles|
|-|-|-|
|**Servicio administrado**|GCP|Memory store, oferta de Redis gestionado|
|**Servicio administrado**|AWS|ElastiCache, oferta de Redis gestionado|
|**Servicio autogestionado**|En las instalaciones|Cluster Redis instalado localmente. [Consulte la documentación específica](redis-cluster/README_es.md)|

En el método **Servicio autogestionado**, la copia de seguridad de Redis se guarda en el directorio ``/data`` de cada host.

## Métodos de Despliegue

En el caso del modelo híbrido, el despliegue puede realizarse de dos maneras, que varían de la carga prevista y la necesidad de una alta disponibilidad. Todos los modos utilizan Docker Compose para realizar la instalación y configuración de la Plataforma.

|Método|Descripción|Escenario Recomendado|Disponibilidad|Carga esperada|
|-|-|-|-|-|
|[**All-In-One**](all-in-one/README_es.md)|Ejecutar todos los módulos en un solo host|Debe ser usado sólo en escenarios POC, nunca en ambientes productivos. No tiene una alta disponibilidad, ni soporta una alta carga. |Baja |Baja
|[**Módulos**](modules/README_es.md)|Contiene todos los módulos segregados. Cada módulo puede funcionar en un host dedicado. |Recomendado para entornos productivos. El dimensionamiento dependerá de la carga esperada.|Alta |Alta

### Módulos

El cuadro siguiente muestra una descripción de los módulos y si se debe o no hacer una copia de seguridad y tener o no un LoadBalancer delante de cada módulo.

|Módulo|Descripción|¿Se requiere una copia de seguridad?
|-|-|-|
|Agent-authorization|Transferencia de escenario entre Cloud Sensedia y Authorization Híbrido. |No|
|Agent-gateway|Transferencia de escenario entre Cloud Sensedia y Gateway Híbrido.|No|
Gateway|Responsable de procesar los mensajes.|No|
Authorization|Responsable de generar tokens.|No|
|Logstash-federated|Transferencia de datos y auditoría de tokens a Cloud Sensedia.|Opcional|
|Redis|Red de memoria para compartir información entre módulos|Sí (usualmente, archivos *.rdb). La copia de seguridad de Redis se guarda en el directorio ``data`` de cada host.

### Recursos Recomendados

Cada servidor debe ser aprovisionado considerando el consumo de la distribución GNU/Linux y otros recursos/servicios que puedan ser instalados.

  ¡¡ATENCIÓN!! El cuadro siguiente exhibe sólo una sugerencia inicial y se considera sólo el consumo mínimo de recursos de hardware para el funcionamiento de los módulos.

  Esta especificación de hardware debe ser cambiada por cada cliente según la demanda de recursos de hardware y de acuerdo con la demanda de uso de los servicios, que se puede observar con el uso diario.

|Servidor|Módulos|CPU|Memoria RAM|Disco|
|-|-|-|-|-|
|API-Gateway 01|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 02|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 03|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|Redis 01|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 02|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 03|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Logstash|Logstash Federado|1|4 GB|100 GB|

### Ejemplos de Deployment

* **All-in-one para carga baja**: 1 instancia (sugerido para un consumo bajo).

		1 instancia que contienen **todos** los módulos + redis-standalone compartiendo recursos.

* **Modules** con 6 instancias para módulos de la Plataforma + 6 instancias para redis-cluster (sugerido para entornos con throughput alto).

		2 instancias dedicadas a Gateway.
		2 instâncias dedicadas a Authorization.
		1 instancia para Agent-gateway y Agent-authorization.
		1 instancia para Logstash-federated.
		6 instancias para Redis cluster.

### Despliegue de Servicios

Para saber más sobre el despliegue usando el método **All-In-One** acceda al archivo [all-in-one/README_es.md](all-in-one/README_es.md).

Para saber más sobre el despliegue usando el método **Modules** acceda al archivo [modules/README_es.md](modules/README_es.md).

## Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros

  ¡¡ATENCIÓN!! Utilice las explicaciones y ejemplo de esta sección como base para cambiar la versión de uno o más módulos de la Plataforma y personalizar los otros parámetros de acuerdo con las necesidades de cada entorno.

A medida que el desarrollo de la Plataforma evoluciona y según las necesidades del entorno híbrido de cada cliente, puede ser necesario personalizar algunos parámetros antes del despliegue.

En cada directorio correspondiente a uno de los métodos de despliegue hay un archivo en el formato ``.yaml`` que instruye a Docker Compose a iniciar uno o más módulos de la Plataforma.

Este es un ejemplo de un archivo ``.yaml`` para desplegar un módulo, seguido de una explicación de los valores que se pueden personalizar.

Ejemplo 1: Contenido del archivo ``agent-gateway.yaml``.

```bash
1   version: '2.4'
2   services:
3
4     api-gateway:
5       env_file: api-gateway.env  # deve cambiar el contenido de este archivo de acuerdo con la documentación
6       networks:
7         - api-platform
8       image: gcr.io/production-main-268117/api-gateway:4.3.0.2 # puede cambiar
9       container_name: api-gateway
10      cpu_count: 1      # puede cambiar
11      mem_limit: 1024m  # puede cambiar
12      restart: always
13      ports:
14        - '8080:8080'   #puede cambiar
15
16  networks:
17    api-platform:
18      name: api-platform
```

Explicación del contenido del archivo ``agent-gateway.yaml``.

* Na **línea 1** se define la versión del archivo Docker Compose. Este valor sólo deve ser cambiado por el equipo de Sensedia cuando sea realmente necesario, pues que afecta a la sintaxis de algunos parámetros y palavras reservadas del archivo, tal y como establece la documentación oficial de Docker Compose: https://docs.docker.com/compose/compose-file/.

* La **línea 2** contiene una palabra reservada de la sintaxis nativa de Docker Compose, que indica el comienzo de un conjunto de instrucciones para desplegar uno o más módulos de la Plataforma.

* La **línea 4** indica el nombre de servicio correspondiente a uno de los módulos de la Plataforma.

* La **línea 5** contiene la ubicación del archivo de variables de entorno. El nombre, la ubicación y el contenido de este archivo cambian según el módulo. **También debe editar este archivo y cambiar los valores que contienen **CHANGE_HERE** a valores consistentes con su entorno híbrido.**

* Las **líneas 6 y 7** contienen una referencia a las **líneas 16 a 18**, que definen una red Docker para ser utilizada por el contenedor que ejecutará este módulo de la Plataforma. No tiene que cambiar el contenido de esta sección.

* La **línea 8** contiene la dirección del Docker Registry (``gcr.io/production-main-268117``), imagen Docker del módulo de la Plataforma (``api-gateway``) y la versão del módulo (``4.3.0.2``). **Debe contactar con el equipo de Sensedia para averiguar qué URL de Docker Registry, nombre de imagen Docker del módulo y la versión que se debe utilizar y cambiar en el archivo ``*.yaml`` antes del despliegue.**

* La **línea 9** contiene el nombre del contenedor que ejecutará el módulo de la Plataforma. No tiene que cambiarla.

* En la **línea 10** se puede definir cuántas CPUs puede usar el servicio que funciona en el contenedor. No todos los módulos contienen este ajuste por defecto.

* En la **línea 11** se puede definir el límite de memoria RAM que puede usar el servicio que funciona en el contenedor. No todos los módulos contienen este ajuste por defecto.

* En la **línea 12** se define la política de reinicio del contenedor. **Se recomienda mantener el valor ``always``** para que el contenedor se reinicie automáticamente en caso de problemas, evitando la necesidad de intervención manual.

* La **línea 13** contiene una palabra reservada de la sintaxis nativa de Docker Compose, que indica el comienzo de una sección para definir los puertos (por defecto, TCP - Transmission Control Protocol) que serán utilizados por el contenedor.

La **línea 13** contiene el puerto que utilizará el host y el contenedor para permitir acceso externo ao módulo de la Plataforma. Los puertos están definidos por la siguiente norma:

PUERTO_HOST:PUERTO_contenedor

Ejemplo 2: ``- '8080:8080'`` - el puerto de host es el 8080/TCP, que escuchará las peticiones y las reenviará al puerto del contenedor, que es el 8080/TCP.

**El puerto de host puede cambiarse** según lo requiera su entorno, pero el puerto del contenedor **no debe cambiarse**.

Ejemplo 3: ``- '80:8080'``. En este caso, el puerto de host es el 80/TCP y el puerto del contenedor es el 8080/TCP.

  ¡¡ATENCIÓN!! El puerto del contenedor no es accesible fuera del hist. Sólo se puede acceder a él dentro del host.

  Todas las peticiones se escuchan en el puerto de host y se transmitem ao puerto de contenedor, de manera transparente para el usuario.

  Cuando configurar las reglas de cortafuegos, considerar sólo los puertos de host.

  El puerto de host está siempre a la izquierda del ``:``.

  El puerto del contenedor está siempre a la derecha del ``:``.

  Por defecto, todos los puertos son TCP.

  El puerto de host puede cambiarse, pero debe ser un puerto diferente cada contenedor que se inicie.

  El puerto del contenedor no puede cambiarse y es definida por el equipo de Sensedia en el momento en que se construye la imagen del Docker.

  Puede haber uno o más puertos de host y de contenedores para cada módulo.

### Creación de Token

La configuración del entorno híbrido tiene como requisito previo el uso de un token de la Plataforma. El token debe ser generado mediante el siguiente procedimiento:

1. Acceder al API Manager;
2. Hacer clic en la opción **Access Token** en el menu principal;
3. Hacer clic en el botón **Create Access token**;
4. El campo **Owner** debe contener el correo electrónico de un usuario responsable del entorno;
5. Establecer el valor **API-Platform Integration** en el campo **APP Field**;
6. En la página siguiente, seleccionar la API **API Manager 3.0.0**;
7. Seleccionar el Plan **Federated Plan**;
8. Publicar el token usando el botón **Publish your access token**;
9. Guardar el token generado (se utilizará en el aprovisionamiento de los módulos).

Cuando el método de instalación sea **All-in-one**:

Editar el archivo ``all-in-one/hybrid.env``, ubicar los siguientes parámetros e sustituir el término ``CHANGE_HERE`` por el token generado anteriormente.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE

Cuando el método de instalación sea **Modules**:

Editar los archivos ``modules/logstash-federated/logstash.env``, ``modules/agent-gateway/agent-gateway.env`` y ``modules/agent-authorization/agent-authorization.env``, ubicar los siguientes parámetros e sustituir el término ``CHANGE_HERE`` por el token generado anteriormente.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE
