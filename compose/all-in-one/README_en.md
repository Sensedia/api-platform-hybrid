<!-- TOC -->

- [All-in-one deployment method](#all-in-one-deployment-method)
  - [Modules](#modules)
  - [Requirements](#requirements)
- [Deploy](#deploy)
  - [Installation](#installation)
  - [Validation](#validation)
  - [Troubleshooting](#troubleshooting)

<!-- TOC END -->

# All-in-one deployment method

This deployment method starts all Platform's stateless modules on the same host.

If you use fully-managed Redis or already have a Redis installation, this might be the ideal model.

## Modules

In this method, the modules mentioned in the section **Modules** of [this page](../README_en.md) must be installed.

## Requirements

Access the section ``Requirements`` of [this page](../README_en.md) to install the dependencies needed for the modules to work.

# Deploy

## Installation

1 - Edit the file ``hybrid.env``, which is inside this directory, and replace the values defines as **CHANGE_HERE** with values consistent with your hybrid environment.

2 - Create an access token for the hybrid environment on the API-Manager following the instructions on [this page]((../README_en.md) (section: **Token Generation**).

3 - Edit the file ``sensedia-all-in-one.yaml`` and modify the versions of the modules, according to the instructions on [this page]((../README_en.md) (section: **Changing Modules Version, Port and Other Parameters**).

4 - Use the following commands to execute ``docker-compose`` referencing the file ``sensedia-all-in-one.yaml``.

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml up -d
```

## Validation

See container status with the following command.

```bash
sudo docker-compose ps
```

This will display a list of services with the respective status.

## Troubleshooting

See the logs of each service with the following command.

```bash
sudo docker-compose logs -f SERVICE_NAME
```

The term ``SERVICE_NAME`` must be replaced with the name of the service whose logs you want to see.

Restart the service with the following command.

```bash
sudo docker-compose restart SERVICE_NAME
```

Stop the service with the following command.

```bash
sudo docker-compose stop SERVICE_NAME
```

Start the service again with the following command

```bash
sudo docker-compose start SERVICE_NAME
```

If you want to stop the service and remove all data, volume, images and container networks, use the following command (only if you really need to).

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml down
```
