<!-- TOC -->

- [Português](#português)
  - [Composição](#composição)
  - [Instalação](#instalação)
  - [Monitoramento, Health Check e Balanceamento](#monitoramento-health-check-e-balanceamento)

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

## Monitoramento, Health Check e Balanceamento

O monitoramento, *health check* e balanceamento de carga no ambiente híbrido é de responsabilidade do cliente, podendo ser utilizadas as ferramentas de sua preferência.

Adicionalmente, disponibilizamos o exporter para Prometheus, que possui informações detalhadas de métricas.

É muito importante utilizar um Load Balancer de sua preferência para que periodicamente seja verificado o estado ou integridade da execução das aplicações. O estado das aplicações é considerado saudável quando o Load Balancer envia requisições HTTP aos endpoints de *health check* das aplicações e obtém o código de retorno 200. Qualquer código de retorno diferente disso, indica que o estado da aplicação não é considirado saudável. Ao perceber que uma aplicação não está saudável, o Load Balancer deve redirecionar o tráfego para outro servidor que esteja executando uma aplicação similar considerada saudável.

Seguem os *endpoints* de métricas e *health check* dos módulos.

Tabela 2: Endpoints de monitoramento dos módulos da plataforma.

| **Módulo** | **Porta Padrão** | **Health check Endpoint** | **Status Code Esperado** | **Métricas para Prometheus** |
| --- | --- | --- | --- | --- |
| Agent Gateway | 8091/TCP | /health | 200 | /metrics |
| Agent Authorization | 8092/TCP | /health | 200 | /metrics |
| API Gateway | 8080/TCP | /gateway-admin/enabled | 200 | /gateway-admin/metrics |
| API Authorization | 8084/TCP | /health | 200 | /metrics |
| Logstash | 8090/TCP, 9600/TCP | / | 200 | N/A |

