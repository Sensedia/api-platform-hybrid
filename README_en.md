<!-- TOC -->

- [English](#english)
  - [Composition](#composition)
  - [Installation](#installation)
  - [Monitoring, health check and load balancing](#monitoring-health-check-and-load-balancing)

<!-- TOC -->

# English

## Composition

Currently, the hybrid environment consists of the following components:

Table 1: Platform modules.

| Module | Description |
|:---|:---|
| **Redis** | Used to store scenarios and tokens on the Platform. It is a component that should be provided preferably by customers. |
| **Agent Authorization** | This module connects with the environment cloud and is responsible for the telemetry of the tokens generated in the client environment |
| **Agent Gateway** | This module connects with the environment cloud and is responsible for receiving all updates to API scenarios |
| **Logstash Federated** | Massively sends the trace generated in the client environment |
| **API Authorization** | Responsible for generating authorization tokens for the APIs |
| **API Gateway** | Receives API requests and performs necessary operations |

## Installation

The installation can be performed using the following technologies:

* [Docker Compose](compose/README_en.md).
* [Kubernetes + Helm](kubernetes/README_en.md).

## Monitoring, health check and load balancing

The client is responsible for health checks, load balancing and monitoring their hybrid environment and can use their preferred tools for that.

Additionally, we provide a Prometheus exporter - Prometheus exhibits thorough metrics information.

It is very important to use a Load Balancer of your choice to periodically check the status or integrity of the applications' execution. The state of the applications is considered healthy when Load Balancer sends HTTP requests to the health check endpoints of the applications and obtains the return code 200. Any return code other than this, indicates that the state of the application is not considered healthy. When realizing that an application is not healthy, Load Balancer should redirect traffic to another server that is running a similar application that is considered healthy.

The following table shows the endpoints of metrics and health check to the modules.

Table 2: Monitoring endpoint for Platform modules.

| **Module** | **Port** | **Health check Endpoint** | **Expected Status Code** | **Prometheus Metrics** |
| --- | --- | --- | --- | --- |
| Agent Gateway | 8091/TCP | /health | 200 | /metrics |
| Agent Authorization | 8092/TCP | /health | 200 | /metrics |
| API Gateway | 8080/TCP | /gateway-admin/enabled | 200 | /gateway-admin/metrics |
| API Authorization | 8084/TCP | /health | 200 | /metrics |
| Logstash | 8090/TCP, 9600/TCP | / | 200 | N/A |
