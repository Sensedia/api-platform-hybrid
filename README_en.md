<!-- TOC -->

- [English](#english)
  - [Composition](#composition)
  - [Installation](#installation)
  - [Monitoring](#monitoring)

<!-- TOC -->

# English

## Composition

Currently, the hybrid environment consists of the following components:

Table 1: Platform modules.

| Module | Description |
|:---|:---|
| **Redis** | Used to store scenarios and tokens on the platform. It is a component that should be provided preferably by customers. |
| **Agent Authorization** | This module connects with the environment cloud and is responsible for the telemetry of the tokens generated in the client environment |
| **Agent Gateway** | This module connects with the environment cloud and is responsible for receiving all updates to API scenarios |
| **Logstash Federated** | Massively sends the trace generated in the client environment |
| **API Authorization** | Responsible for generating authorization tokens in the APIs |
| **API Gateway** | Receives API requests and performs necessary operations |

## Installation

The installation can be performed using the following technologies:

* [Docker Compose](compose/README_en.md).
* [Kubernetes + Helm](kubernetes/README_en.md).

## Monitoring

The client is responsible for monitoring their hybrid environment and can use their preferred tools.

Additionally, we provide a Prometheus exporter - Prometheus exhibits thorough metrics information.

The following table shows the endpoints to apply monitoring to the modules.

Table 2: Monitoring endpoint for Platform modules.

| **Module** | **Port** | **Endpoint** | **Expected Status Code** | **Prometheus Metrics** |
| --- | --- | --- | --- | --- |
| Agent Gateway | 8091/TCP | /health | 200 | /metrics |
| Agent Authorization | 8092/TCP | /health | 200 | /metrics |
| API Gateway | 8080/TCP | /gateway-admin/enabled | 200 | /gateway-admin/metrics |
| API Authorization | 8084/TCP | /health | 200 | /metrics |
| Logstash | 8090/TCP, 9600/TCP | / | 200 | N/A |

