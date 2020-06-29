<!-- TOC -->

- [Hybrid API-Platform - Docker Compose](#hybrid-api-platform-docker-compose)
- [Start the Docker service](#start-the-docker-service)
- [Configure Docker to boot up with the OS](#configure-docker-to-boot-up-with-the-os)
- [Add your user to the Docker group](#add-your-user-to-the-docker-group)
		- [Docker Compose](#docker-compose)
		- [Selinux on CentOS/Red Hat](#selinux-on-centosred-hat)
		- [Redis](#redis)
	- [Deployment Methods](#deployment-methods)
		- [Modules](#modules)
		- [Recommended Resources](#recommended-resources)

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->





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

You can find more information on the official documentation: https://docs.docker.com/compose/install/.

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
|**Self-managed service**|On Premises|Redis cluster installed locally. [Consult specific documentation](redis-cluster/README.md)|

For the **Self-managed service** method, the Redis backup is saved in the ``/data`` directory of each host.

## Deployment Methods

Regarding the hybrid model, deployment can be done in three ways, which vary according to the expected load and the need for high availability. All modes use Docker Compose to carry out installation and configuration actions.

|Method|Description|Recommended Scenario|Availability|Expected Load|
|-|-|-|-|-|
|[**All-In-One**](all-in-one/README.md)|Runs Restful modules (all modules except Redis)|Can be used for production scenarios of light and/or moderate load, as long as all modules run on more than one host, so that there is high availability.|Moderate|Moderate
|[**Poc-all-in-one**](poc-all-in-one/README.md)|Executes all modules on a single host|Must only be used in POC scenarios, never in productive environments. Does not have high availability, nor supports heavy load. |Low|Light|
|[**Modules**](modules/README.md)|Contains all segregated modules. Each module can run on a dedicated host. |Recommended for productive environments. Sizing will depend on the expected load. |High|Heavy|

### Modules

The table below contains a description of the modules and whether there is a need for backup and a Load Balancer in front of each module.

|Module|Description|Is backup needed?|
|-|-|-|
|Agent-authorization|Scenery transfer between Cloud Sensedia and Hybrid Authorization.|No|
|Agent-gateway|Scenery transfer between Cloud Sensedia and Hybrid Gateway.|No|
|Gateway|Responsible for processing messages.|No|
|Authorization|Responsible for token generation.|No|
|Logstash-federated|Transfer of analytical data and token audit for Cloud Sensedia.|Optional|No|
|Redis|Memory grid for information sharing among modules|Yes (usually *.rdb files). Redis' backup is saved in the ``/data`` directory of each host.|

### Recommended Resources

Each server must be provisioned considering the performance of the GNU/Linux distribution and other resources/services that may be installed.

	ATTENTION!!! The following table shows only an initial suggestion and considers only the resource use of minimum hardware for the modules to run.

	These hardware specs must be altered by each client according to hardware resource needs and demand for services, to be observed with daily use.

|Server|Módulos|CPU|RAM Memory|Hard disk|
|-|-|-|-|-|
|API-Gateway 01|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 02|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 03|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|Redis 01|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 02|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 03|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Logstash|Federated Logstash|1|4 GB|100 GB|

### Deployment Examples

* **All-in-one for moderate load**: 3 instances for Platform modules + 3 instances for Redis cluster (moderate use).

		3 instances containing **all** modules **(except Redis)** sharing resources.
		3 instances for Redis providing availability in case of failure.

* **All-in-one for light load**: 3 instances (recommended for low consumption).

		3 instances containing **all** modules + redis-cluster sharing resources.

* **Modules** with 6 instances for for Platform modules + 6 instances for redis-cluster (recommended for environments with high throughput).

		2 instâncias dedicadas para Gateway.
		2 instâncias dedicadas para Authorization.
		1 instância para Agent-gateway e Agent-authorization.
		1 instância para Logstash-federated.
		6 instâncias para Redis cluster.
