<!-- TOC -->

- [Português](#português)
  - [Ambientes Híbridos do API-Platform](#ambientes-híbridos-do-api-platform)
  - [Necessidade](#necessidade)
  - [Lista de Conteúdos](#lista-de-conteúdos)
- [English](#english)
  - [Hybrid Environments](#hybrid-environments)
  - [Need](#need)
  - [List of contents](#list-of-contents)
- [Español](#español)
  - [Entornos Híbridos de API-Platform](#entornos-híbridos-de-api-platform)
  - [Necesidad](#necesidad)
  - [Lista de contenidos](#lista-de-contenidos)

<!-- TOC -->

 <p align="center">
    <img src="images/api-management-platform-sensedia-features-apis.png" alt="API-Platform">
 </p>

# Português

**API-Platform**: plataforma de gerenciamento de APIs. Acelere suas estratégias digitais com integrações e gerenciamento de APIs. Saiba em: https://sensedia.com.

## Ambientes Híbridos do API-Platform

Ambientes híbridos do API-Platform são ambientes onde apenas uma parte dos componentes da solução operam remotamente (no cliente), diferente do que acontece em ambientes cloud (gerenciados pela Sensedia).

## Necessidade

O ambiente híbrido é necessário quando o cliente deseja realizar operações de APIs, internamente em sua própria infraestrutura.

Nesse caso, existe a possibilidade de que alguns componentes da topologia do API-Platform operem remotamente, mas ainda assim mantenham comunicação com os módulos core na cloud.

## Lista de Conteúdos

<!-- TOC -->

- [Composição](README_pt.md#composição)
- [Instalação](README_pt.md#instalação)
  - [Usando Docker Compose](compose/README.md#api-platform-híbrido---docker-compose)
    - [Requisitos](compose/README.md#requisitos)
      - [Docker](compose/README.md#docker)
      - [Docker-Compose](compose/README.md#docker-compose)
      - [Selinux no CentOS/Red Hat](compose/README.md#selinux-no-centosred-hat)
      - [Redis](compose/README.md#redis)
        - [Redis Cluster](compose/redis-cluster/README.md#redis-cluster)
          - [Deploy Redis Cluster Utilizando 6 Nodes (full mode)](compose/redis-cluster/README.md#deploy-redis-cluster-utilizando-6-nodes-full-mode)
          - [Deploy Redis Cluster Utilizando 3 Nodes (minimal mode)](compose/redis-cluster/README.md#deploy-redis-cluster-utilizando-3-nodes-minimal-mode)
            - [Instalando o Redis no Node 1](compose/redis-cluster/README.md#instalando-o-redis-no-node-1)
            - [Instalando o Redis no Node 2](compose/redis-cluster/README.md#instalando-o-redis-no-node-2)
            - [Instalando o Redis no Node 3](compose/redis-cluster/README.md#instalando-o-redis-no-node-3)
            - [Configurando o Redis Cluster](compose/redis-cluster/README.md#configurando-o-redis-cluster)
            - [Comandos Úteis](compose/redis-cluster/README.md#comandos-úteis)
          - [Backup](compose/redis-cluster/README.md#backup)
    - [Métodos de Deploy](compose/README.md#métodos-de-deploy)
      - [Módulos](compose/README.md#módulos)
      - [Recursos Recomendados](compose/README.md#recursos-recomendados)
      - [Exemplos de Deployment](compose/README.md#exemplos-de-deployment)
      - [Deploy dos Serviços](compose/README.md#deploy-dos-serviços)
        - [Método all-in-one](compose/all-in-one/README.md#all-in-one-deployment-method)
          - [Instalação](compose/all-in-one/README.md#instalação)
          - [Validação](compose/all-in-one/README.md#validação)
          - [Troubleshooting](compose/all-in-one/README.md#troubleshooting)
        - [Método Segmented modules](compose/modules/README.md#api-platform-híbrido---docker-compose-modules)
            - [Instalação](compose/modules/README.md#instalação)
              - [Logstash](compose/modules/README.md#logstash)
              - [Agent-Authorization](compose/modules/README.md#agent-authorization)
              - [Agent-Gateway](compose/modules/README.md#agent-gateway)
              - [API-Authorization](compose/modules/README.md#api-authorization)
              - [API-Gateway](compose/modules/README.md#api-gateway)
            - [Validação](compose/modules/README.md#validação)
            - [Troubleshooting](compose/modules/README.md#troubleshooting)
    - [Alterando a Versão dos Módulos, Porta e Outros Parâmetros](compose/README.md#alterando-a-versão-dos-módulos-porta-e-outros-parâmetros)
      - [Criação de Token](compose/README.md#criação-de-token)
  - [Usando Kubernetes](kubernetes/README.md#api-platform-híbrido---kubernetes)
    - [Módulos para Ambiente Híbrido](kubernetes/README.md#módulos-para-ambiente-híbrido)
    - [Modelos de Deployment Suportados](kubernetes/README.md#modelos-de-deployment-suportados)
    - [Topologia Macro](kubernetes/README.md#topologia-macro)
    - [Requisitos de Instalação](kubernetes/README.md#requisitos-de-instalação)
      - [Criação do Customer ID](kubernetes/README.md#criação-do-customer-id)
      - [Criação de Token](kubernetes/README.md#criação-de-token)
      - [Redis](kubernetes/README.md#redis)
        - [AWS ElastiCache](kubernetes/README.md#aws-elasticache)
        - [GCP Memorystore](kubernetes/README.md#gcp-memorystore)
        - [Instalação do Redis com Docker Compose](kubernetes/README.md#instalação-do-redis-com-docker-compose)
      - [Instalação do Kubectl](kubernetes/README.md#instalação-do-kubectl)
      - [Instalação do Helm](kubernetes/README.md#instalação-do-helm)
        - [Download do Helm](kubernetes/README.md#download-do-helm)
        - [Repositório de Helm Charts da Sensedia](kubernetes/README.md#repositório-de-helm-charts-da-sensedia)
      - [Criação de Namespace no Cluster Kubernetes](kubernetes/README.md#criação-de-namespace-no-cluster-kubernetes)
    - [Comandos Úteis](kubernetes/README.md#comandos-úteis)
    - [Instalação dos Módulos do API-Platform](kubernetes/README.md#instalação-dos-módulos-do-api-platform)
      - [Alterando a Versão dos Módulos e Outros Parâmetros](kubernetes/README.md#alterando-a-versão-dos-módulos-e-outros-parâmetros)
      - [Instalação do Agent Authorization](kubernetes/README.md#instalação-do-agent-authorization)
      - [Instalação do Agent Gateway](kubernetes/README.md#instalação-do-agent-gateway)
      - [Instalação do Logstash-Federated](kubernetes/README.md#instalação-do-logstash-federated)
      - [Instalação do API-Authorization](kubernetes/README.md#instalação-do-api-authorization)
      - [Instalação do API-Gateway](kubernetes/README.md#instalação-do-api-gateway)
    - [Ativação de Ambiente Híbrido](kubernetes/README.md#ativação-de-ambiente-híbrido)
- [Monitoramento](README_pt.md#monitoramento)

<!-- TOC -->

# English

**API-Platform**: API management platform. Accelerate your digital strategies with integrations and API Management. Learn more at: https://sensedia.com

## Hybrid Environments

Hybrid environments are environments where only a part of the solution components operate remotely (on the client), differently from what happens in cloud environments (managed by Sensedia).

## Need

The hybrid environment is necessary when the customer wants to perform API operations internally, in their own infrastructure.

In this case, there is a possibility that some components of the API-Platform topology operate remotely, but still maintain communication with the core modules on the cloud.

## List of contents

- [Composition](README_en.md#composition)
- [Installation](README_en.md#installation)
  - [Using Docker Compose](compose/README_en.md#hybrid-api-platform---docker-compose)
    - [Requirements](compose/README_en.md#requirements)
      - [Docker](compose/README_en.md#docker)
      - [Docker Compose](compose/README_en.md#docker-compose)
      - [Selinux on CentOS/Red Hat](compose/README_en.md#selinux-on-centosred-hat)
      - [Redis](compose/README_en.md#redis)
        - [Redis Cluster](compose/redis-cluster/README_en.md#redis-cluster)
          - [Redis Cluster Deployment Using 6 Nodes (full mode)](compose/redis-cluster/README_en.md#redis-cluster-deployment-using-6-nodes-full-mode)
          - [Redis Cluster Deployment Using 3 Nodes (minimal mode)](compose/redis-cluster/README_en.md#redis-cluster-deployment-using-3-nodes-minimal-mode)
            - [Installing Redis on Node 1](compose/redis-cluster/README_en.md#installing-redis-on-node-1)
            - [Installing Redis on Node 2](compose/redis-cluster/README_en.md#installing-redis-on-node-2)
            - [Installing Redis on Node 3](compose/redis-cluster/README_en.md#installing-redis-on-node-3)
            - [Configuring the Redis Cluster](compose/redis-cluster/README_en.md#configuring-the-redis-cluster)
            - [Useful Commands](compose/redis-cluster/README_en.md#useful-commands)
          - [Backup](compose/redis-cluster/README_en.md#backup)
    - [Deployment Methods](compose/README_en.md#deployment-methods)
      - [Modules](compose/README_en.md#modules)
      - [Recommended Resources](compose/README_en.md#recommended-resources)
      - [Deployment Examples](compose/README_en.md#deployment-examples)
      - [Deploying the services](compose/README_en.md#deploying-the-services)
        - [All-in-one method](compose/all-in-one/README_en.md#all-in-one-deployment-method)
          - [Installation](compose/all-in-one/README_en.md#installation)
          - [Validation](compose/all-in-one/README_en.md#validation)
          - [Troubleshooting](compose/all-in-one/README_en.md#troubleshooting)
        - [Segmented modules method](compose/modules/README_en.md#api-platform-híbrido---docker-compose-modules)
          - [Installation](compose/modules/README_en.md#installation)
            - [Logstash](compose/modules/README_en.md#logstash)
            - [Agent-Authorization](compose/modules/README_en.md#agent-authorization)
            - [Agent-Gateway](compose/modules/README_en.md#agent-gateway)
            - [API-Authorization](compose/modules/README_en.md#api-authorization)
            - [API-Gateway](compose/modules/README_en.md#api-gateway)
          - [Validation](compose/modules/README_en.md#validation)
          - [Troubleshooting](compose/modules/README_en.md#troubleshooting)
    - [Changing Modules Version, Port and Other Parameters](compose/README_en.md#changing-modules-version-port-and-other-parameters)
      - [Token Generation](compose/README_en.md#token-generation)
  - [Using Kubernetes](kubernetes/README_en.md#hybrid-api-platform---kubernetes)
    - [Hybrid Environment Modules](kubernetes/README_en.md#hybrid-environment-modules)
    - [Supported Deployment Modelos](kubernetes/README_en.md#supported-deployment-modelos)
    - [Macro Topology](kubernetes/README_en.md#macro-topology)
    - [Installation Requirements](kubernetes/README_en.md#installation-requirements)
      - [Customer ID creation](kubernetes/README_en.md#customer-id-creation)
      - [Token creation](kubernetes/README_en.md#token-creation)
      - [Redis](kubernetes/README_en.md#redis)
        - [AWS ElastiCache](kubernetes/README_en.md#aws-elasticache)
        - [GCP Memorystore](kubernetes/README_en.md#gcp-memorystore)
        - [Installing Redis with Docker Compose](kubernetes/README_en.md#installing-redis-with-docker-compose)
      - [Installing Kubectl](kubernetes/README_en.md#installing-kubectl)
      - [Installing Helm](kubernetes/README_en.md#installing-helm)
        - [Downloading Helm](kubernetes/README_en.md#downloading-helm)
        - [Sensedia Helm Charts Repository](kubernetes/README_en.md#sensedia-helm-charts-repository)
      - [Creation of Namespace on the Kubernetes Cluster](kubernetes/README_en.md#creation-of-namespace-on-the-kubernetes-cluster)
    - [Useful Commands](kubernetes/README_en.md#useful-commands)
    - [Installation of the API-Platform Modules](kubernetes/README_en.md#installation-of-the-api-platform-modules)
      - [Changing Modules Versions and Other Parameters](kubernetes/README_en.md#changing-modules-versions-and-other-parameters)
      - [Installing Agent Authorization](kubernetes/README_en.md#installing-agent-authorization)
      - [Installing Agent Gateway](kubernetes/README_en.md#installing-agent-gateway)
      - [Installing Logstash-Federated](kubernetes/README_en.md#installing-logstash-federated)
      - [Installing API-Authorization](kubernetes/README_en.md#installing-api-authorization)
      - [Installing API-Gateway](kubernetes/README_en.md#installing-api-gateway)
    - [Hybrid Environment Activation](kubernetes/README_en.md#hybrid-environment-activation)
- [Monitoring](README_en.md#monitoring)


# Español

**API-Platform**: plataforma de administración de APIs. Acelere sus estrategias digitales con integraciones y API Management. Obtenga más informaciónes en: https://sensedia.com.

## Entornos Híbridos de API-Platform

Los entornos híbridos de API-Platform son entornos en los que solo una parte de los componentes de la solución funcionan de forma remota (en el cliente), a diferencia de lo que sucede en entornos de nube (administrados por Sensedia).

## Necesidad

El entorno híbrido es necesario cuando el cliente desea realizar operaciones de APIs internamente, en su propia infraestructura.

En este caso, existe la posibilidad de que algunos componentes de la topología de la API-Platform operen de forma remota, pero aún mantengan comunicación con los módulos principales en la nube.

## Lista de contenidos

- [Composición](README_es.md#composición)
- [Instalación](README_es.md#instalación)
  - [Utilizando Docker Compose](compose/README_es.md#api-platform-híbrido---docker-compose)
    - [Requerimientos](compose/README_es.md#requerimientos)
    - [Docker](compose/README_es.md#docker)
      - [Docker-Compose](compose/README_es.md#docker-compose)
      - [Selinux en CentOS/Red Hat](compose/README_es.md#selinux-en-centosred-hat)
      - [Redis](compose/README_es.md#redis)
        - [Redis Cluster](compose/redis-cluster/README_es.md#redis-cluster)
          - [Requisitos](compose/redis-cluster/README_es.md#requisitos)
          - [Despliegue de Redis Cluster Utilizando 6 Nodos (full mode)](compose/redis-cluster/README_es.md#despliegue-de-redis-cluster-utilizando-6-nodos-full-mode)
          - [Despliegue de Redis Cluster Utilizando 3 Nodos (minimal mode)](compose/redis-cluster/README_es.md#despliegue-de-redis-cluster-utilizando-3-nodos-minimal-mode)
            - [Instalación de Redis en Node 1](compose/redis-cluster/README_es.md#instalación-de-redis-en-node-1)
            - [Instalación de Redis en Node 2](compose/redis-cluster/README_es.md#instalación-de-redis-en-node-2)
            - [Instalación de Redis en Node 3](compose/redis-cluster/README_es.md#instalación-de-redis-en-node-3)
            - [Configuración del Redis Cluster](compose/redis-cluster/README_es.md#configuración-del-redis-cluster)
            - [Comandos Útiles](compose/redis-cluster/README_es.md#comandos-útiles)
          - [Copia de Seguridad](compose/redis-cluster/README_es.md#copia-de-seguridad)
    - [Métodos de Despliegue](compose/README_es.md#métodos-de-despliegue)
      - [Módulos](compose/README_es.md#módulos)
      - [Recursos Recomendados](compose/README_es.md#recursos-recomendados)
      - [Ejemplos de Deployment](compose/README_es.md#ejemplos-de-deployment)
      - [Despliegue de Servicios](compose/README_es.md#despliegue-de-servicios)
        - [Método all-in-one](compose/all-in-one/README_es.md#método-de-despliegue-all-in-one)
          - [Instalación](compose/all-in-one/README_es.md#instalación)
          - [Validación](compose/all-in-one/README_es.md#validación)
          - [Solución de problemas](compose/all-in-one/README_es.md#solución-de-problemas)
        - [Método Segmented modules](compose/modules/README_es.md#api-platform-híbrido---docker-compose-modules)
          - [Instalación](compose/modules/README_es.md#instalación)
            - [Logstash](compose/modules/README_es.md#logstash)
            - [Agent-Authorization](compose/modules/README_es.md#agent-authorization)
            - [Agent-Gateway](compose/modules/README_es.md#agent-gateway)
            - [API-Authorization](compose/modules/README_es.md#api-authorization)
            - [API-Gateway](compose/modules/README_es.md#api-gateway)
          - [Validación](compose/modules/README_es.md#validación)
          - [Solución de problemas](compose/modules/README_es.md#solución-de-problemas)
    - [Cambio de Versión de los Módulos, Puerto de Red y Otros Parámetros](compose/README_es.md#cambio-de-versión-de-los-módulos-puerto-de-red-y-otros-parámetros)
      - [Creación de Token](compose/README_es.md#creación-de-token)
  - [Utilizando Kubernetes](kubernetes/README_es.md#api-platform-híbrido---kubernetes)
    - [Módulos para Entorno Híbrido](kubernetes/README_es.md#módulos-para-entorno-híbrido)
    - [Modelos de Despliegue Soportados](kubernetes/README_es.md#modelos-de-despliegue-soportados)
    - [Topología Macro](kubernetes/README_es.md#topología-macro)
    - [Requisitos de Instalación](kubernetes/README_es.md#requisitos-de-instalación)
      - [Creación de Customer ID](kubernetes/README_es.md#creación-de-customer-id)
      - [Creación de Tokens](kubernetes/README_es.md#creación-de-tokens)
      - [Redis](kubernetes/README_es.md#redis)
        - [AWS ElastiCache](kubernetes/README_es.md#aws-elasticache)
        - [GCP Memorystore](kubernetes/README_es.md#gcp-memorystore)
        - [Instalación de Redis con Docker Compose](kubernetes/README_es.md#instalación-de-redis-con-docker-compose)
      - [Instalación de Kubectl](kubernetes/README_es.md#instalación-de-kubectl)
      - [Instalación del Helm](kubernetes/README_es.md#instalación-del-helm)
        - [Descarga de Helm](kubernetes/README_es.md#descarga-de-helm)
        - [Repositorio de Helm Charts de Sensedia](kubernetes/README_es.md#repositorio-de-helm-charts-de-sensedia)
      - [Creación de Namespace en el Clúster Kubernetes](kubernetes/README_es.md#creación-de-namespace-en-el-clúster-kubernetes)
    - [Comandos Útiles](kubernetes/README_es.md#comandos-útiles)
    - [Instalación de los Módulos de la API-Platform](kubernetes/README_es.md#instalación-de-los-módulos-de-la-api-platform)
      - [Cambio de la Versión de los Módulos y Otros Parámetros](kubernetes/README_es.md#cambio-de-la-versión-de-los-módulos-y-otros-parámetros)
      - [Instalación de Agent Authorization](kubernetes/README_es.md#instalación-de-agent-authorization)
      - [Instalación de Agent Gateway](kubernetes/README_es.md#instalación-de-agent-gateway)
      - [Instalación de Logstash-Federated](kubernetes/README_es.md#instalación-de-logstash-federated)
      - [Instalación de API-Authorization](kubernetes/README_es.md#instalación-de-api-authorization)
      - [Instalación de API-Gateway](kubernetes/README_es.md#instalación-de-api-gateway)
    - [Activación del Entorno Híbrido](kubernetes/README_es.md#activación-del-entorno-híbrido)
- [Seguimiento](README_es.md#seguimiento)