<!-- TOC -->

- [Español](#español)
  - [Composición](#composición)
  - [Instalación](#instalación)
  - [Monitoreo, Health Check y Balanceo](#monitoreo-health-check-y-balanceo)

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

## Monitoreo, Health Check y Balanceo

El monitoreo, *health check* y balanceo de carga del entorno híbrido es responsabilidad del cliente y se pueden utilizar las herramientas de su elección.

Además, proporcionamos el exportador de Prometheus, que exhibe información métrica detallada.

Es muy importante utilizar un Load Balancer de su elección para comprobar periódicamente el estado o la integridad de la ejecución de las aplicaciones. El estado de las aplicaciones se considera saludable cuando Load Balancer envía solicitudes HTTP a los puntos finales de * verificación de estado * de las aplicaciones y obtiene el código de retorno 200. Cualquier código de retorno distinto de este indica que el estado de la aplicación no se considera saludable. Al darse cuenta de que una aplicación no está en buen estado, Load Balancer debe redirigir el tráfico a otro servidor que esté ejecutando una aplicación similar que se considere en buen estado.

A continuación se presentan los endpoints de las métricas y health check de los módulos.

Cuadro 2: Endpoints de monitoreo de los módulos de la Plataforma.

| **Módulo** | **Puerta** | **Health check Endpoint** | **Código de Estado Esperado** | **Métricas para Prometheus** |
| --- | --- | --- | --- | --- |
| Agent Gateway | 8091/TCP | /health | 200 | /metrics |
| Agent Authorization | 8092/TCP | /health | 200 | /metrics |
| API Gateway | 8080/TCP | /gateway-admin/enabled | 200 | /gateway-admin/metrics |
| API Authorization | 8084/TCP | /health | 200 | /metrics |
| Logstash | 8090/TCP, 9600/TCP | / | 200 | N/A |
