<!-- TOC -->

- [Português](#português)
  - [Composição](#composição)
  - [Instalação](#instalação)
  - [Monitoramento](#monitoramento)

<!-- TOC -->

# Português

## Composição

Atualmente, o ambiente híbrido é composto pelos seguintes componentes:

Tabela 1: Módulos da plataforma.

| Módulo | Descrição |
|:---|:---|
| **Redis** | Utilizado para armazenamento de cenários e tokens na plataforma. É um componente que deverá ser fornecido preferencialmente pelos clientes. |
| **Agent Authorization** | Esse módulo se conecta com o ambiente cloud e é responsável pelas telemetria dos tokens gerados no ambiente do cliente. |
| **Agent Gateway** | Esse módulo se conecta com o ambiente cloud e é responsável por receber todas as atualizações de cenários de APIs. |
| **Logstash Federated** | Faz o envio massivo do trace gerado no ambiente do cliente. |
| **API Authorization** | Responsável pela geração de tokens de autorização nas APIs. |
| **API Gateway** | Recebe as requisições de APIs e realiza as operações necessárias. |

## Instalação

A instalação pode ser realizada utilizando as seguintes tecnologias:

* [Docker Compose](compose/README.md).
* [Kubernetes + Helm](kubernetes/README.md).

## Monitoramento

O monitoramento do ambiente híbrido é de responsabilidade do cliente, podendo ser utilizadas as ferramentas de sua preferência.

Adicionalmente, disponibilizamos o exporter para Prometheus, que possui informações detalhadas de métricas.

Seguem os endpoints para aplicação do monitoramento dos módulos.

Tabela 2: Endpoints de monitoramento dos módulos da plataforma.

| **Módulo** | **Porta Padrão** | **Endpoint** | **Status Code Esperado** | **Métricas para Prometheus** |
| --- | --- | --- | --- | --- |
| Agent Gateway | 8091/TCP | /health | 200 | /metrics |
| Agent Authorization | 8092/TCP | /health | 200 | /metrics |
| API Gateway | 8080/TCP | /gateway-admin/enabled | 200 | /gateway-admin/metrics |
| API Authorization | 8084/TCP | /health | 200 | /metrics |
| Logstash | 8090/TCP, 9600/TCP | / | 200 | N/A |

