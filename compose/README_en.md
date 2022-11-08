<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [Hybrid API-Platform - Docker Compose](#hybrid-api-platform---docker-compose)
	- [Requirements](#requirements)
		- [Docker](#docker)
		- [Docker Compose](#docker-compose)
		- [Selinux on CentOS/Red Hat](#selinux-on-centosred-hat)
		- [Redis](#redis)
	- [Deployment Methods](#deployment-methods)
		- [Modules](#modules)
		- [Recommended Resources](#recommended-resources)
		- [Deployment Examples](#deployment-examples)
		- [Deploying the services](#deploying-the-services)
	- [Changing Modules Version, Port and Other Parameters](#changing-modules-version-port-and-other-parameters)
		- [Token Generation](#token-generation)
<!-- TOC END -->




# Hybrid API-Platform - Docker Compose

The **Hybrid** deployment model is recommended for clients with latency concerns.

It is often used for **On-Premise internal backends**, when there is no point for gateways to be in the cloud.

 This document explains how to deploy modules/services used in a hybrid environment.

## Requirements

 * Docker 18.09 or higher
 * Docker Compose 1.18 or higher
 * Redis 4.0.11 or higher
 * Host without internet access restrictions (proxy not supported)

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
ATTENTION!!! The ``docker-compose`` command may not be found after installation. If this happens, run the following command to create a symbolic link:

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
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
|**Self-managed service**|On Premises|Redis cluster installed locally. [Consult specific documentation](redis-cluster/README_en.md)|

For the **Self-managed service** method, the Redis backup is saved in the ``/data`` directory of each host.

## Deployment Methods

Regarding the hybrid model, deployment can be done in two ways, which vary according to the expected load and the need for high availability. All modes use Docker Compose to carry out installation and configuration actions.

|Method|Description|Recommended Scenario|Availability|Expected Load|
|-|-|-|-|-|
|[**All-In-One**](all-in-one/README_en.md)|Executes all modules on a single host|Must only be used in POC scenarios, never in productive environments. Does not have high availability, nor supports heavy load. |Low|Light|
|[**Modules**](modules/README_en.md)|Contains all segregated modules. Each module can run on a dedicated host. |Recommended for productive environments. Sizing will depend on the expected load. |High|Heavy|

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

|Server|Modules|CPU|RAM Memory|Hard disk|
|-|-|-|-|-|
|API-Gateway 01|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 02|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 03|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|Redis 01|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 02|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 03|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Logstash|Federated Logstash|1|4 GB|100 GB|

### Deployment Examples

* **All-in-one for light load**: 1 instance (recommended for low consumption).

		1 instance containing **all** modules + redis-standalone sharing resources.

* **Modules** with 6 instances for for Platform modules + 6 instances for redis-cluster (recommended for environments with high throughput).

		2 instâncias dedicadas para Gateway.
		2 instâncias dedicadas para Authorization.
		1 instância para Agent-gateway e Agent-authorization.
		1 instância para Logstash-federated.
		6 instâncias para Redis cluster.

### Deploying the services

To know more about deployment using the method **All-In-One**, check this file: [all-in-one/README_en.md](all-in-one/README_en.md).

To know more about deployment using the method **Modules** check this file: [modules/README_en.md](modules/README_en.md).

## Changing Modules Version, Port and Other Parameters

	ATTENTION!!! Use this section's explanation and example as a basis when altering the version of one or more modules and customising other parameters according to the needs of each environment.

As the development of the Platform evolves and according to the needs of each client's hybrid environment, some parameters might need to be customised before deployment.

In each directory corresponding to every deployment method there is a ``.yaml`` file that instructs Docker Compose to start one or more modules of the Platform.

This is an example of a ``.yaml`` file to deploy a module, followed by an explanation of which values can be customised.

Example 1: Content of file ``agent-gateway.yaml``.

```bash
1   version: '2.4'
2   services:
3
4     api-gateway:
5       env_file: api-gateway.env  # you must alter the content of this file according to the documentation
6       networks:
7         - api-platform
8       image: gcr.io/production-main-268117/api-gateway:4.3.0.2 # you can alter
9       container_name: api-gateway
10      cpu_count: 1      # you can alter
11      mem_limit: 1024m  # you can alter
12      restart: always
13      ports:
14        - '8080:8080'   #you can alter
15
16  networks:
17    api-platform:
18      name: api-platform
```

Explanation of the content of the file ``agent-gateway.yaml``.

* **Line 1** defines the Docker Compose file version. This value must be altered by the Sensedia team only when absolutely necessary, since it affects the syntax of some parameters and reserved words in the file, as the Docker Compose official documentation shows: https://docs.docker.com/compose/compose-file/.

* **Line 2** contains a reserved word of native Docker Compose syntax, which indicates the beginning of a set of instructions to deploy one or more Platform modules.

* **Line 4** indicates the service name corresponding to one of the Platform modules.

* **Line 5** contain the location of the environment variables file. The name, location and content of this file change according to the module. **You must also edit this file and alter the values defined as **CHANGE_HERE** to values consistent with your hybrid environment.**

* **Lines 6 and 7** contain a reference to **lines 16 to 18**, which define the Docker network to be used by the container that will execute this Platform module. You don't have to change the content of this section.

* **Line 8** contains the Docker Registry (``gcr.io/production-main-268117``), Docker image of the Platform module (``api-gateway``) and module version (``4.3.0.2``). **You must get in touch with the Sensedia team to know which Docker Registry URL, module Docker image name and version you must use and modify in the ``*.yaml`` file before deployment.**

* **Line 9** contains the name of the container which will execute the Platform module. You don't need to change it.

* On **line 10**, you can define how many CPUs the service running on the container can use. Not all modules have this definition as a default.

* On **line 11**, you can define the RAM memory limit that the service running on the container can use. Not all modules have this definition as a default.

* **Line 12** defines the container restart policy. **We recommend setting the value to ``always``** so that the container restarts automatically in case of problems, thus avoiding the need for manual intervention.

* **Line 13** contains a reserved word of native Docker Compose syntax, which indicates the beginning of a section to define the ports (TCP - Transmission Control Protocol - by default) that will be used by the container.

* **Line 14** contains the port to be used by the host and by the container to allow external access to the Platform module. The ports are defined following this pattern:

PORT_HOST:PORT_container

Example 2: ``- '8080:8080'`` - the host port is 8080/TCP, which will listen to requests and forward them to the container port, which is 8080/TCP.

**The host port can be changed** as required by your environment, but the container port **should not be changed**.

Example 3 ``- '80:8080'``. In this case, the host port is 80/TCP and the container port is 8080/TCP.

	ATTENTION!!! The container port is inaccessible from outside the host. It's only accessible from inside the host.

	All requests are listened to on the host port and forwarded to the container port, and that is transparent for the user.

	When configuring firewall rules, consider only the host ports.

	The host port is always to the left of ``:``.

	The container port is always to the right of ``:``.

	By default, all ports are TCP.

	The host port may be altered, but there must be a different port for each container to be started.

	The container port must not be altered and it's defined by the Sensedia team when the Docker image is being built.

	There may be one or more host and container ports for each module.

### Token Generation

The configuration of the hybrid environment has as a prerequisite the usage of a Platform token. The token must be generated following this procedure:

1. Access the API Manager;
2. Click the **Consumers** --> **Access Tokens** option on the main menu;
3. Click the **Create Access token** button;
4. The field **Owner** must contain the email of a user responsible for the environment;
5. Define the value **API-Platform Integration** on the field **APP Field**;
6. On the following page, select the API **API Manager 3.0.0**;
7. Select the Plan **Federated Plan**;
8. Publish the token by clicking the button **Publish your access token**;
9. Save the token generated (it will be used to provision the modules).

When the installation method is **All-in-one**:

Edit the file ``all-in-one/hybrid.env``, find the following parameters and replace the term ``CHANGE_HERE`` with the token generated previously.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE

When the installation method is **Modules**:

Edit the files ``modules/logstash-federated/logstash.env``, ``modules/agent-gateway/agent-gateway.env`` and ``modules/agent-authorization/agent-authorization.env``, find the following parameters and replace the term ``CHANGE_HERE`` with the token generated previously.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE
