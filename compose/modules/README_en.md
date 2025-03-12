<!-- TOC -->

- [Hybrid API-Platform - Docker Compose Modules](#hybrid-api-platform---docker-compose-modules)
  - [Requirements](#requirements)
- [Deploy](#deploy)
  - [Installation](#installation)
    - [Logstash](#logstash)
    - [Agent-Authorization](#agent-authorization)
    - [Agent-Gateway](#agent-gateway)
    - [API-Authorization](#api-authorization)
    - [API-Gateway](#api-gateway)
  - [Validation](#validation)
  - [Troubleshooting](#troubleshooting)

<!-- TOC -->

# Hybrid API-Platform - Docker Compose Modules

In this method, each module can be instanced separately.

## Requirements

Access the **Requirements** section of [this page](../README.md) to read on how to install the dependencies for the modules to run.

# Deploy

## Installation

### Logstash

* Edit the file ``logstash-federated/logstash.env`` and replace the values defines as **CHANGE_HERE** with values consistent with your environment.

* Edit the file ``logstash-federated/logstash-federated.yaml`` and change the module version, according to the instructions on the **Changing Modules Version, Port and Other Parameters** section of [this page](../README.md).

* Execute the service using ``docker-compose`` with the following command.

```bash
sudo docker-compose -f logstash-federated/logstash-federated.yaml up -d
```

### Agent-Authorization

* Edit the file ``agent-authorization/agent-authorization.env`` and replace the values defines as **CHANGE_HERE** with values consistent with your environment.

* Create an access token for the hybrid environment on API-Manager following the instruction on the **Token Generation** section of [this page](../README.md).

* Edit the file ``agent-authorization/agent-authorization.yaml`` and change the module version, according to the instructions on the **Changing Modules Version, Port and Other Parameters** section of [this page](../README.md).

* Execute the service using ``docker-compose`` with the following command.

```bash
sudo docker-compose -f agent-authorization.yaml up -d
```

### Agent-Gateway

* Edit the file ``agent-gateway/agent-gateway.env`` and replace the values defines as **CHANGE_HERE** with values consistent with your environment.

* Create an access token for the hybrid environment on API-Manager following the instruction on the **Token Generation** section of [this page](../README.md).

* Edit the file ``agent-gateway/agent-gateway.yaml`` and change the module version, according to the instructions on the **Changing Modules Version, Port and Other Parameters** section of [this page](../README.md).

* Execute the service using ``docker-compose`` with the following command.

```bash
sudo docker-compose -d -f agent-gateway/agent-gateway.yaml up
```

### API-Authorization

* Edit the file ``api-authorization/api-authorization.env`` and replace the values defines as **CHANGE_HERE** with values consistent with your environment.

* Edit the file ``api-authorization/api-authorization.yaml`` and change the module version, according to the instructions on the **Changing Modules Version, Port and Other Parameters** section of [this page](../README.md).

* Execute the service using ``docker-compose`` with the following command.

```bash
sudo docker-compose -f api-authorization/api-authorization.yaml up -d
```

### API-Gateway

* Edit the file ``api-gateway/api-gateway.env`` and replace the values defines as **CHANGE_HERE** with values consistent with your environment.

* Edit the file ``api-gateway/api-gateway.yaml`` and change the module version, according to the instructions on the **Changing Modules Version, Port and Other Parameters** section of [this page](../README.md).

* Execute the service using ``docker-compose`` with the following command.

```bash
sudo docker-compose -f api-gateway/api-gateway.yaml up -d
```

## Validation

See container status with the following command.

```bash
sudo docker-compose -f DIR_NAME/COMPOSE_FILE_NAME ps
```

Replace the term ``DIR_NAME`` with the service directory.

Replace the term ``COMPOSE_FILE_NAME`` with the service file.

A list of services with the respective status will be displayed.

## Troubleshooting

See the logs of each service with the following commands.

```bash
cd DIR_NAME
sudo docker-compose logs -f SERVICE_NAME
```

Replace the term ``DIR_NAME`` with the service directory.

The term ``SERVICE_NAME`` must be replaced with the name of the service whose logs you want to see.

Restart the service with the following commands.

```bash
cd DIR_NAME
sudo docker-compose restart SERVICE_NAME
```

Stop the service with the following commands.

```bash
cd DIR_NAME
sudo docker-compose stop SERVICE_NAME
```

Start the service again with the following commands.

```bash
cd DIR_NAME
sudo docker-compose start SERVICE_NAME
```

If you want to stop the service and remove all data, volume, images and container networks, use the following command (only if you really need to).

```bash
cd DIR_NAME
sudo docker-compose -f SERVICE_NAME.yaml down
```
* Validate your API by making a request to the hybrid gateway; Access this link for documentation on [Validation](../validation/README_pt.md).
