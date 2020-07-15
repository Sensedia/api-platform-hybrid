











# POC-all-in-one deployment method

This deployment method starts all Platform modules on the same host.

    ATTENTION!!! This method should not be used in the production environment.

## Modules

In this method, the modules mentioned on the section **Modules** of [this page](../README_en.md) must be installed.

## Requirements

Access the **Requirements** section of [this page](../README.md) to read on how to install the dependencies for the modules to run.

# Deploy

## Installation

1 - Edit the file ``hybrid.env``, which is inside this directory, and replace the values defines as **CHANGE_HERE** with values consistent with your hybrid environment.

2 - Create an access token for the hybrid environment on the API-Manager following the instructions on [this page]((../README_en.md) (section: **Token Generation**).

3 - Edit the file ``poc-all-in-one.yaml`` and modify the versions of the modules, according to the instructions on [this page]((../README_en.md) (section: **Changing Modules Version, Port and Other Parameters**).

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

Start the service again with the following command.

```bash
sudo docker-compose start SERVICE_NAME
```

If you want to stop the service and remove all data, volume, images and container networks, use the following command (only if you really need to).

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml down
```
