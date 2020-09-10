<!-- TOC -->

- [Redis Cluster](#redis-cluster)
  - [Requisitos](#requisitos)
  - [Despliegue de Redis Cluster Utilizando 6 Nodos (full mode)](#despliegue-de-redis-cluster-utilizando-6-nodos-full-mode)
  - [Despliegue de Redis Cluster Utilizando 3 Nodos (minimal mode)](#despliegue-de-redis-cluster-utilizando-3-nodos-minimal-mode)
    - [Instalación de Redis en Node 1](#instalación-de-redis-en-node-1)
    - [Instalación de Redis en Node 2](#instalación-de-redis-en-node-2)
    - [Instalación de Redis en Node 3](#instalación-de-redis-en-node-3)
    - [Configuración del Redis Cluster](#configuración-del-redis-cluster)
    - [Comandos Útiles](#comandos-útiles)
  - [Copia de Seguridad](#copia-de-seguridad)

<!-- TOC -->

# Redis Cluster

La documentación oficial de Redid sugiere un setup con **6 nodos** dedicados. Ejemplo:

|node_1|node_2|node_3|node_4|node_5|node_6|
|-|-|-|-|-|-|
|redis-a1|redis-b1|redis-c1|redis-b2|redis-c2|redis-a2|

En este escenario, nodos de tipo **1** son **nodos maestros** y nodos de tipo **2** son **nodos secundarios**.  

Este despliegue también puede reducirse a **3 nodos**, con un mayor riesgo de pérdida de datos, conformando este escenario:

|node_1|node_2|node_3|
|-|-|-|
|groupA|groupB|groupC|
|redis-a1 + redis-c2|redis-b1 + redis-c2|redis-c1 + redis-b2|

En caso de que uno de los hosts no esté disponible, el servicio seguirá estando disponible y podrá recuperarse completamente después de que el host se haya recuperado.

## Requisitos

Acceder a la sección **Requisitos** en [esta página](../README.md) para obtener información sobre como instalar las dependencias para el funcionamiento de los módulos.

## Despliegue de Redis Cluster Utilizando 6 Nodos (full mode)

// En cursor

## Despliegue de Redis Cluster Utilizando 3 Nodos (minimal mode)

### Instalación de Redis en Node 1

Copiar el archivo ``minimal/group_redis/group.yaml`` para ``redis_groupA.yaml`` en al host ``node_1``.

Ejecutar los siguientes comandos para iniciar el **redis-master** y el **redis-replica** del grupo A.

```bash
sudo docker-compose -f redis_groupA.yaml up -d
```

### Instalación de Redis en Node 2

Copiar el archivo ``minimal/group_redis/group.yaml`` para ``redis_groupB.yaml`` en al host ``node_2``.

Ejecutar los siguientes comandos para iniciar el **redis-master** y el **redis-replica** del grupo B.

```bash
sudo docker-compose -f redis_groupB.yaml up -d
```

### Instalación de Redis en Node 3

Copiar el archivo ``minimal/group_redis/group.yaml`` para ``redis_groupC.yaml`` en al host ``node_1``.

Ejecutar los siguientes comandos para iniciar el **redis-master** y el **redis-replica** del grupo C.

```bash
sudo docker-compose -f redis_groupC.yaml up -d
```

### Configuración del Redis Cluster

Ejecutar el siguiente comando en el host ``node_1``(y confirmar).

```bash
sudo docker run -i --rm --entrypoint redis-cli redis:5.0.3 --cluster create node_1:6379 node_2:6379 node_3:6379 node_1:6380 node_3:6380 node_2:6380 --cluster-replicas 1
```

### Comandos Útiles

Consultar a los registros con el siguiente comando.

```bash
sudo docker-compose -f redis_group{ID} logs -f
```

Sustituir el término ``{ID}`` por el numero del nodo.

Parar el servicio con el siguiente comando.

```bash
sudo docker-compose -f redis_group{ID} down
```

## Copia de Seguridad

La copia de seguridad de Redis se salva en el directorio ``/data`` de cada host.
