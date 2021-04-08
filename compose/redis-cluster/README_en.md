<!-- TOC -->

- [Redis Cluster](#redis-cluster)
  - [Requirements](#requirements)
  - [Redis Cluster Deployment Using 6 Nodes (full mode)](#redis-cluster-deployment-using-6-nodes-full-mode)
  - [Redis Cluster Deployment Using 3 Nodes (minimal mode)](#redis-cluster-deployment-using-3-nodes-minimal-mode)
    - [Installing Redis on Node 1](#installing-redis-on-node-1)
    - [Installing Redis on Node 2](#installing-redis-on-node-2)
    - [Installing Redis on Node 3](#installing-redis-on-node-3)
    - [Configuring the Redis Cluster](#configuring-the-redis-cluster)
    - [Useful Commands](#useful-commands)
  - [Backup](#backup)

<!-- TOC -->

# Redis Cluster

Redis' official documentation (https://redis.io/topics/cluster-tutorial) recommends a setup with **6** dedicated **nodes**. Example:

|node_1|node_2|node_3|node_4|node_5|node_6|
|-|-|-|-|-|-|
|redis-a1|redis-b1|redis-c1|redis-b2|redis-c2|redis-a2|

In this scenario, type **1** nodes are **master nodes** and type **2** nodes are **slave nodes**.

This deployment can also be reduced to **3 nodes**, at a greater risk of losing data, conforming this scenario:

|node_1|node_2|node_3|
|-|-|-|
|groupA|groupB|groupC|
|redis-a1 + redis-c2|redis-b1 + redis-c2|redis-c1 + redis-b2|

If one the hosts becomes unavailable, the service will still be available and able to completely recover after the host is back on.

## Requirements

Access the **Requirements** section of [this page](../README.md) to read on how to install the dependencies for the modules to run.

## Redis Cluster Deployment Using 6 Nodes (full mode)

// In progress

## Redis Cluster Deployment Using 3 Nodes (minimal mode)

### Installing Redis on Node 1

Copy the file ``minimal/group_redis/group.yaml`` to ``redis_groupA.yaml`` on the host ``node_1``.

Execute the following commands to start group A's **redis-master** and **redis-replica**.

```bash
sudo docker-compose -f redis_groupA.yaml up -d
```

### Installing Redis on Node 2

Copy the file ``minimal/group_redis/group.yaml`` to ``redis_groupB.yaml`` on the host ``node_2``.

Execute the following commands to start group B's **redis-master** and **redis-replica**.

```bash
sudo docker-compose -f redis_groupB.yaml up -d
```

### Installing Redis on Node 3

Copy the file ``minimal/group_redis/group.yaml`` to ``redis_groupC.yaml`` on the host ``node_3``.

Execute the following commands to start group C's **redis-master** and **redis-replica**.

```bash
sudo docker-compose -f redis_groupC.yaml up -d
```

### Configuring the Redis Cluster

Execute the following command on host ``node_1`` (and confirm).

```bash
sudo docker run -i --rm --network host --entrypoint redis-cli redis:5.0.3 --cluster create node_1:6379 node_2:6379 node_3:6379 node_1:6380 node_3:6380 node_2:6380 --cluster-replicas 1
```

### Useful Commands

Consult logs with the following command.

```bash
sudo docker-compose -f redis_group{ID} logs -f
```

The term ``{ID}`` must be replaced with the node number.

Stop the service with the following command.

```bash
sudo docker-compose -f redis_group{ID} down
```

## Backup

O backup do Redis é salvo no diretório ``/data`` de cada host.
