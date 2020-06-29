



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
|**Servicio autogestionado**|En las instalaciones|Cluster Redis instalado localmente. [Consulte la documentación específica](redis-cluster/README.md)|

En el método **Servicio autogestionado**, la copia de seguridad de Redis se guarda en el directorio ``/data`` de cada host.

## Métodos de Despliegue

En el caso del modelo híbrido, el despliegue puede realizarse de três maneras, que varían de la carga prevista y la necesidad de una alta disponibilidad. Todos los modos utilizan Docker Compose para realizar la instalación y configuración de la Plataforma.

|Método|Descripción|Escenario Recomendado|Disponibilidad|Carga esperada|
|-|-|-|-|-|
|[**All-In-One**](all-in-one/README.md)|Ejecuta los módulos Restful (todos los módulos excepto Redis)|Puede ser usado para la producción en escenarios de carga baja y/o moderada, siempre y cuando todos los módulos funcionen en más de un host, para que haya alta disponibilidad. |Moderada |Moderada
|[**Poc-all-in-one**](poc-all-in-one/README.md)|Ejecutar todos los módulos en un solo host|Debe ser usado sólo en escenarios POC, nunca en ambientes productivos. No tiene una alta disponibilidad, ni soporta una alta carga. |Baja |Baja
|[**Módulos**](modules/README.md)|Contiene todos los módulos segregados. Cada módulo puede funcionar en un host dedicado. |Recomendado para entornos productivos. El dimensionamiento dependerá de la carga esperada.|Alta |Alta

### Módulos

La siguiente tabla muestra una descripción de los módulos y si se debe o no hacer una copia de seguridad y tener o no un LoadBalancer delante de cada módulo.

|Módulo|Descripción|¿Se requiere una copia de seguridad?
|-|-|-|
|Agent-authorization|Transferencia de escenario entre Cloud Sensedia y Authorization Híbrido. |No|
|Agent-gateway|Transferencia de escenario entre Cloud Sensedia y Gateway Híbrido.|No|
Gateway|Responsable de procesar los mensajes.|No|
Authorization|Responsable de generar tokens.|No|
|Logstash-federated|Transferencia de datos y auditoría de tokens a Cloud Sensedia.|Opcional|
|Redis|Red de memoria para compartir información entre módulos|Sí (usualmente, archivos *.rdb). La copia de seguridad de Redis se guarda en el directorio ``data`` de cada host.

### Recursos Recomendados
