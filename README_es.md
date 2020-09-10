<!-- TOC -->

- [Español](#español)
  - [Composición](#composición)
  - [Instalación](#instalación)
  - [Seguimiento](#seguimiento)

<!-- TOC -->

# Español

## Composición

Actualmente, el entorno híbrido consta de los siguientes componentes:

Cuadro 1: Módulos de la Plataforma.

| Módulo | Descripción |
|:---|:---|
| **Redis** | Se usa para almacenar escenarios y tokens en la plataforma. Es un componente que debe ser proporcionado preferentemente por los clientes. |
| **Agent Authorization** | Este módulo se conecta con el entorno de la nube y es responsable de la telemetría de los tokens generados en el entorno del cliente. |
| **Agent Gateway** | Este módulo se conecta con el entorno de la nube y es responsable de recibir todas las actualizaciones de los escenarios API.
| **Logstash Federated** | Envía masivamente la traza generada en el entorno del cliente. |
| **API Authorization** | Responsable de generar tokens de autorización en las APIs.
| **API Gateway** | Recibe peticiones de APIs y realiza las operaciones necesarias. |

## Instalación

La instalación se puede realizar utilizando las siguientes tecnologías:

* [Docker Compose](compose/README_es.md).
* [Kubernetes + Helm](kubernetes/README_es.md).

## Seguimiento

El seguimiento del entorno híbrido es responsabilidad del cliente y se pueden utilizar las herramientas de su elección.

Además, proporcionamos el exportador de Prometheus, que exhibe información métrica detallada.

A continuación se presentan los endpoints para aplicar el seguimiento de los módulos.

Cuadro 2: Endpoints de seguimiento de los módulos de la Plataforma.

| **Módulo** | **Puerta** | **Endpoint** | **Código de Estado Esperado** | **Métricas para Prometheus** |
| --- | --- | --- | --- | --- |
| Agent Gateway | 8091/TCP | /health | 200 | /metrics |
| Agent Authorization | 8092/TCP | /health | 200 | /metrics |
| API Gateway | 8080/TCP | /gateway-admin/enabled | 200 | /gateway-admin/metrics |
| API Authorization | 8084/TCP | /health | 200 | /metrics |
| Logstash | 8090/TCP, 9600/TCP | / | 200 | N/A |
