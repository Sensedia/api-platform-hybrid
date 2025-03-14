<!-- TOC -->

- [All-in-one deployment method](#all-in-one-deployment-method)
  - [Módulos](#módulos)
  - [Requisitos](#requisitos)
- [Deploy](#deploy)
  - [Instalação](#instalação)
  - [Validação](#validação)
  - [Troubleshooting](#troubleshooting)

<!-- TOC -->

# All-in-one deployment method

Este método de deployment inicia todos os módulos *stateless* da plataforma no mesmo host.

> ATENÇÃO!!! Esse método não deve ser utilizado no ambiente de produção. Este método é recomendado apenas para demontrações, testes ou PoC (*Proof of concept*).

## Módulos

Neste método devem ser instalados os módulos citados [nessa página](../README.md), na seção **Módulos**.

## Requisitos

Acesse [essa página](../README.md), na seção ``Requisitos``, para obter informações sobre como instalar as dependências para o funcionamento dos módulos.

# Deploy

## Instalação

1 - Edite o arquivo ``hybrid.env``, contido neste diretório, e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente.

2 - Crie um token de acesso do ambiente híbrido no API-Manager seguindo as instruções [dessa página](../README.md), na seção **Criação de Token**.

3 - Edite o arquivo ``sensedia-all-in-one.yaml`` e altere as versões dos módulos, conforme as instruções [dessa página](../README.md), na seção **Alterando a Versão dos Módulos, Porta e Outros Parâmetros**.

4 - Use os comandos a seguir para executar o ``docker-compose`` referenciando o arquivo ``sensedia-all-in-one.yaml``.

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml up -d
```

## Validação

Veja o status dos contêineres com o seguinte comando.

```bash
sudo docker-compose ps
```

A lista de serviços será exibida com o respectivo status.

## Troubleshooting

Veja os logs de cada serviço com o seguinte comando.

```bash
sudo docker-compose logs -f SERVICE_NAME
```

O termo ``SERVICE_NAME`` deve ser substituído pelo nome do serviço cujo log você deseja visualizar.

Reinicie o serviço com o seguinte comando.

```bash
sudo docker-compose restart SERVICE_NAME
```

Pare o serviço com o seguinte comando.

```bash
sudo docker-compose stop SERVICE_NAME
```

Inicie novamente o serviço com o seguinte comando.

```bash
sudo docker-compose start SERVICE_NAME
```

Se quiser parar o serviço e remover todos os dados, volumes, imagens, rede dos contêineres, use o comando a seguir (use apenas se realmente precisar).

```bash
cd compose/all-in-one/
sudo docker-compose -f sensedia-all-in-one.yaml down
```
* Efetue o teste de validação de sua API fazendo uma requisição no Gateway Híbrido; Acesse este link para documentação de [Validação](../validation/README_pt.md).
