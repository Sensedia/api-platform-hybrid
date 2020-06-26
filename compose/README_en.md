


















# Hybrid API-Platform - Docker Compose

The **Hybrid** deployment model is recommended for clients with latency concerns.

It is often used for **On-Premise internal backends**, when there is no point for gateways to be in the cloud.

 This document explains how to deploy modules/services used in a hybrid environment.

 ## Requirements

 * Docker 18.09 or higher
 * Docker Compose 1.18 or higher
 * Redis 4.0.11 or higher

 ### Docker

Install Docker CE (Community Edition) following the instructions of the pages below, according to your GNU/Linux distribution.

* CentOS:
https://docs.docker.com/install/linux/docker-ce/centos/

* Debian:
https://docs.docker.com/install/linux/docker-ce/debian/

* Ubuntu Server:
https://docs.docker.com/install/linux/docker-ce/ubuntu/

Start the Docker service, configure Docker to boot up with the OS and add your user to the Docker group.

```bash
# Start the Docker service
sudo systemctl start docker

# Configure Docker to boot up with the OS
sudo systemctl enable docker

# Add your user to the Docker group
sudo usermod -aG docker $USER
sudo setfacl -m user:$USER:rw /var/run/docker.sock
```

Source: https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot

### Docker Compose

Install Docker Compose with the following commands

```bash
COMPOSE_VERSION=1.25.5

sudo curl -L "https://github.com/docker/compose/releases/download/$COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

/usr/local/bin/docker-compose version
```

You can find more information on the official documentation:
https://docs.docker.com/compose/install/

### Selinux on CentOS/Red Hat

Selinux can interfere on file and process permissions. To avoid problems, disable it with the following commands.

```bash
sudo setenforce 0
```

Edit the file ``/etc/sysconfig/selinux`` and change the following parameter.

Before:

```
SELINUX=enforcing
```

After:

```
SELINUX=disabled
```

Restart the host to apply the alteration.

Source: https://www.hostinger.com.br/tutoriais/desabilitar-selinux-centos-7/

### Redis

There are many options to run Redis.

|Deployment method|Environment|Details|
|-|-|-|
|**Managed service**|GCP|Memory store, managed Redis offer|
|**Manager service**|AWS|ElastiCache, managed Redis offer|
|**Self-managed service**|On Premises|Redis cluster installed locally. [Consult specific documentação](redis-cluster/README.md)|

For the **Self-managed service** method, the Redis backup is saved in the ``/data`` directory of each host.

## Deployment Methodist
