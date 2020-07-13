<!-- TOC -->

- [API-Platform Híbrido - Docker Compose Modules](#api-platform-híbrido---docker-compose-modules)
	- [Requisitos](#requisitos)
- [Deploy](#deploy)
	- [Instalação](#instalação)
		- [Logstash](#logstash)
		- [Agent-Authorization](#agent-authorization)
		- [Agent-Gateway](#agent-gateway)
		- [API-Authorization](#api-authorization)
		- [API-Gateway](#api-gateway)
	- [Validação](#validação)
	- [Troubleshooting](#troubleshooting)

<!-- TOC -->

# API-Platform Híbrido - Docker Compose Modules

Neste método, cada módulo pode ser instanciado separadamente.

## Requisitos

Acesse [essa página](../README.md), na seção **Requisitos**, para obter informações sobre como instalar as dependências para o funcionamento dos módulos.

# Deploy

## Instalação

### Logstash

* Edite o arquivo ``logstash-federated/logstash.env`` e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente.

* Edite o arquivo ``logstash-federated/logstash-federated.yaml`` e altere a versão do módulo, conforme as instruções [dessa página](../README.md), na seção **Alterando a Versão dos Módulos, Porta e Outros Parâmetros**.

* Execute o serviço usando o ``docker-compose`` com o seguinte comando.

```bash
sudo docker-compose -f logstash-federated/logstash-federated.yaml up -d
```

### Agent-Authorization

* Edite o arquivo ``agent-authorization/agent-authorization.env`` e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente.

* Crie um token de acesso do ambiente híbrido no API-Manager seguindo as instruções [dessa página](../README.md), na seção **Criação de Token**.

* Edite o arquivo ``agent-authorization/agent-authorization.yaml`` e altere a versão do módulo, conforme as instruções [dessa página](../README.md), na seção **Alterando a Versão dos Módulos, Porta e Outros Parâmetros**.

* Execute o serviço usando o ``docker-compose`` com o seguinte comando.

```bash
sudo docker-compose -f agent-authorization.yaml up -d
```

### Agent-Gateway

* Edite o arquivo ``agent-gateway/agent-gateway.env`` e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente.

* Crie um token de acesso do ambiente híbrido no API-Manager seguindo as instruções [dessa página](../README.md), na seção **Criação de Token**.

* Edite o arquivo ``agent-gateway/agent-gateway.yaml`` e altere a versão do módulo, conforme as instruções [dessa página](../README.md), na seção **Alterando a Versão dos Módulos, Porta e Outros Parâmetros**.

* Execute o serviço usando o ``docker-compose`` com o seguinte comando.

```bash
sudo docker-compose -d -f agent-gateway/agent-gateway.yaml up
```

### API-Authorization

* Edite o arquivo ``api-authorization/api-authorization.env`` e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente.

* Edite o arquivo ``api-authorization/api-authorization.yaml`` e altere a versão do módulo, conforme as instruções [dessa página](../README.md), na seção **Alterando a Versão dos Módulos, Porta e Outros Parâmetros**.

* Execute o serviço usando o ``docker-compose`` com o seguinte comando.

```bash
sudo docker-compose -f api-authorization/api-authorization.yaml up -d
```

### API-Gateway

* Edite o arquivo ``api-gateway/api-gateway.env`` e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente.

* Edite o arquivo ``api-gateway/api-gateway.yaml`` e altere a versão do módulo, conforme as instruções [dessa página](../README.md), na seção **Alterando a Versão dos Módulos, Porta e Outros Parâmetros**.

* Execute o serviço usando o ``docker-compose`` com o seguinte comando.

```bash
sudo docker-compose -f api-gateway/api-gateway.yaml up -d
```

## Validação

Veja o status dos containers com o seguinte comando.

```bash
sudo docker-compose -f NOME_DIR/NOME_COMPOSE_FILE ps
```

Altere o termo ``NOME_DIR`` pelo diretório do serviço.

Altere o termo ``NOME_COMPOSE_FILE`` nome do arquivo do serviço.

A lista de serviços será exibida com o respectivo status.

## Troubleshooting

Veja os logs de cada serviço com os seguintes comandos.

```bash
cd NOME_DIR
sudo docker-compose logs -f SERVICE_NAME
```

Altere o termo ``NOME_DIR`` pelo diretório do serviço.

O termo ``SERVICE_NAME`` deve ser substituído pelo nome do serviço cujo log você deseja visualizar.

Reinicie o serviço com os seguintes comandos.

```bash
cd NOME_DIR
sudo docker-compose restart SERVICE_NAME
```

Pare o serviço com os seguintes comandos.

```bash
cd NOME_DIR
sudo docker-compose stop SERVICE_NAME
```

Inicie novamente os serviços com os seguintes comandos.

```bash
cd NOME_DIR
sudo docker-compose start SERVICE_NAME
```

Se quiser parar o serviço e remover todos os dados, volumes, imagens, rede dos contêineres, use os comandos a seguir (use apenas se realmente precisar).

```bash
cd NOME_DIR
sudo docker-compose -f SERVICE_NAME.yaml down
```
