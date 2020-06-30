<!-- TOC -->

- [API-Platform Híbrido - Docker Compose](#api-platform-híbrido---docker-compose)
  - [Requisitos](#requisitos)
    - [Docker](#docker)
    - [Docker-Compose](#docker-compose)
    - [Selinux no CentOS/Red Hat](#selinux-no-centosred-hat)
    - [Redis](#redis)
  - [Métodos de Deploy](#métodos-de-deploy)
    - [Módulos](#módulos)
    - [Recursos Recomendados](#recursos-recomendados)
    - [Exemplos de Deployment](#exemplos-de-deployment)
    - [Deploy dos Serviços](#deploy-dos-serviços)
  - [Alterando a Versão dos Módulos, Porta e Outros Parâmetros](#alterando-a-versão-dos-módulos-porta-e-outros-parâmetros)
    - [Criação de Token](#criação-de-token)

<!-- TOC -->


# API-Platform Híbrido - Docker Compose

O modelo de deploy **Híbrido** é recomendado para clientes que têm preocupação com latência.

Muitas vezes é usado para conexões de **backends internos On-Premise**, assim não faz sentido os gateways estarem localizados na nuvem.

Esta documentação explica como realizar o deployment dos módulos/serviços utilizados no ambiente híbrido.

## Requisitos

* Docker 18.09 ou versão superior
* Docker Compose 1.18 ou versão superior
* Redis 4.0.11 ou versão superior

### Docker

Instale o Docker CE (Community Edition) seguindo as instruções das páginas a seguir de acordo com a distribuição GNU/Linux.

* No CentOS:
https://docs.docker.com/install/linux/docker-ce/centos/

* No Debian:
https://docs.docker.com/install/linux/docker-ce/debian/

* No Ubuntu Server:
https://docs.docker.com/install/linux/docker-ce/ubuntu/

Inicie o serviço Docker, configure o Docker para ser inicializado no boot do S.O e adicione o seu usuário ao grupo Docker.

```bash
# Inicie o serviço Docker
sudo systemctl start docker

# Configure o Docker para ser inicializado no boot do S.O
sudo systemctl enable docker

# Adicione o seu usuário ao grupo Docker
sudo usermod -aG docker $USER
sudo setfacl -m user:$USER:rw /var/run/docker.sock
```

Fonte: https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot

### Docker-Compose

Instale o Docker Compose com os seguintes comandos.

```bash
COMPOSE_VERSION=1.25.5

sudo curl -L "https://github.com/docker/compose/releases/download/$COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

/usr/local/bin/docker-compose version
```

Maiores informações podem ser obtidas na documentação oficial: https://docs.docker.com/compose/install/.

### Selinux no CentOS/Red Hat

O Selinux pode interferir no permissionamento de arquivos e processos. Para evitar problemas, desabilite-o com os seguintes comandos.

```bash
sudo setenforce 0
```

Edite o arquivo ``/etc/sysconfig/selinux`` e mude o seguinte parâmetro.

Antes:

```
SELINUX=enforcing
```

Depois:

```
SELINUX=disabled
```

Reinicie o host para aplicar a alteração.

Fonte: https://www.hostinger.com.br/tutoriais/desabilitar-selinux-centos-7/

### Redis

Há várias opções para executar o Redis.

|Método de deploy|Ambiente|Detalhes|
|-|-|-|
|**Managed service**|GCP|Memory store, oferta de Redis gerenciado|
|**Manager service**|AWS|ElastiCache, oferta de Redis gerenciado|
|**Self-managed service**|On Premises|Redis cluster instalado localmente. [Consulte a documentação específica](redis-cluster/README.md)|

No método **Self-managed service**, o backup do Redis é salvo no diretório ``/data`` de cada host.

## Métodos de Deploy

Para o modelo Híbrido o deploy pode ser realizado de três modos, variando em relação a carga esperada e a necessidade de alta disponibilidade. Todos os modos usam Docker Compose para realizar a instalação e configuração da plataforma.

|Método|Descrição|Cenário recomendado|Disponibilidade|Carga Esperada|
|-|-|-|-|-|
|[**All-In-One**](all-in-one/README.md)|Executa os módulos Restful (todos os módulos, exceto Redis)|Pode ser utilizado para produção em cenários de carga baixa e/ou moderada, desde que todos os módulos sejam executados em mais de um host, para que haja alta disponibilidade.|Média|Média
|[**Poc-all-in-one**](poc-all-in-one/README.md)|Executa todos os módulos em um único host|Deve ser utilizado somente em cenário de POC, nunca, em ambientes produtivos. Não conta com alta disponibilidade, nem suporta carga alta.|Baixa|Baixa|
|[**Modules**](modules/README.md)|Contém todos os módulos segregados. Cada módulo pode ser executado em um host dedicado.|Recomendado para ambientes produtivos. O dimensionamento dependerá da carga prevista.|Alta|Alta|

### Módulos

A tabela a seguir mostra uma descrição dos módulos e da necessidade de fazer ou não backup e de ter ou não um LoadBalancer na frente de cada módulo.

|Módulo|Descrição|Necessário backup?|
|-|-|-|
|Agent-authorization|Transferência de cenário entre Cloud Sensedia e Authorization Híbrido.|Não|
|Agent-gateway|Transferência de cenário entre Cloud Sensedia e Gateway Híbrido.|Não|
|Gateway|Responsável por processar as mensagens.|Não|
|Authorization|Responsável pela geração de tokens.|Não|
|Logstash-federated|Transferência de dados analíticos e auditoria de tokens para Cloud Sensedia.|Opcional|Não|
|Redis|Grid de memória para compartilhamento de informações entre os módulos|Sim (normalmente dos arquivos *.rdb). O backup do Redis é salvo no diretório ``/data`` de cada host.|

### Recursos Recomendados

Cada servidor deve ser provisionado considerando o consumo da distribuição GNU/Linux e demais recursos/serviços que possam ser instalados.

    ATENÇÃO!!! A tabela a seguir expressa apenas uma sugestão inicial e considera apenas o consumo de recursos de hardware mínimo para o funcionamento dos módulos.

    Essa especificação de hardware deve ser alterada por cada cliente de acordo com a demanda por recursos de hardware e de acordo com a demanda para utilização dos serviços, a ser observada com o uso diário.

|Servidor|Módulos|CPU|Memória RAM|Disco|
|-|-|-|-|-|
|API-Gateway 01|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 02|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|API-Gateway 03|Agent Gateway<br>Agent Authorization<br>API Gateway<br>API Authorization|2|4 GB|60 GB|
|Redis 01|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 02|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Redis 03|Redis Master<br>Redis Slave|2|4 GB|60 GB|
|Logstash|Logstash Federado|1|4 GB|100 GB|

### Exemplos de Deployment

* **All-in-one para carga moderada**: 3 instâncias para módulos da plataforma + 3 instâncias para Redis cluster (uso moderado).

		3 instâncias contendo **todos** os módulos **(menos Redis)** compartilhando recursos.
		3 instâncias para Redis provendo disponibilidade em caso de falhas.

* **All-in-one para carga baixa**: 3 instâncias (recomendado para baixo consumo).

		3 instâncias contendo **todos** os módulos + redis-cluster compartilhando recursos.

* **Modules** com 6 instâncias para módulos da plataforma + 6 instâncias para redis-cluster (recomendado para ambientes com alto throughput).

		2 instâncias dedicadas para Gateway.
		2 instâncias dedicadas para Authorization.
		1 instância para Agent-gateway e Agent-authorization.
		1 instância para Logstash-federated.
		6 instâncias para Redis cluster.

### Deploy dos Serviços

Para saber mais informações sobre o deploy usando o método **All-In-One** acesse o arquivo [all-in-one/README.md](all-in-one/README.md).

Para saber mais informações sobre o deploy usando o método **Poc-all-in-one** acesse o arquivo [poc-all-in-one/README.md](poc-all-in-one/README.md).

Para saber mais informações sobre o deploy usando o método **Modules** acesse o arquivo [modules/README.md](modules/README.md).

## Alterando a Versão dos Módulos, Porta e Outros Parâmetros

    ATENÇÃO!!! Use as explicações desta seção e o exemplo como base para alterar a versão de um ou mais módulos da plataforma e customizar os demais parâmetros conforme a necessidade de cada ambiente.

À medida que o desenvolvimento da plataforma evolui e de acordo com as necessidades do ambiente híbrido de cada cliente, pode ser necessário customizar alguns parâmetros antes de fazer o deploy.

Em cada diretório correspondente a um dos métodos de deploy existe um arquivo no formato ``.yaml`` que instrui o Docker Compose a iniciar um ou mais módulos da plataforma.

A seguir é mostrado o exemplo de um arquivo ``.yaml`` para deploy de um módulo e explicado que valores podem ser customizados.

Exemplo 1: Conteúdo do arquivo ``agent-gateway.yaml``.

```bash
1   version: '2.4'
2   services:
3
4     api-gateway:
5       env_file: api-gateway.env  # você deve alterar o conteúdo desse arquivo conforme a documentação
6       networks:
7         - api-platform
8       image: gcr.io/production-main-231423/api-gateway:4.2.0.0 # você pode alterar
9       container_name: api-gateway
10      cpu_count: 1      # você pode alterar
11      mem_limit: 1024m  # você pode alterar
12      restart: always
13      ports:
14        - '8080:8080'   #você pode alterar
15
16  networks:
17    api-platform:
18      name: api-platform
```

Explicação do conteúdo do arquivo ``agent-gateway.yaml``.

* Na **linha 1** é definida a versão do Docker Compose file. Este valor deve ser alterado apenas pelo time da Sensedia quando realmente for necessário, pois afeta a sintaxe de alguns parâmetros e palavras reservadas do arquivo, conforme mostra a documentação oficial do Docker Compose: https://docs.docker.com/compose/compose-file/.

* A **linha 2** contém um palavra reservada da sintaxe nativa do Docker Compose, que indica o início de um conjunto de instruções para deploy de um ou mais módulos da plataforma.

* A **linha 4** indica o nome de serviço correspondente a um dos módulos da plataforma.

* A **linha 5** contém a localização do arquivo de variáveis de ambiente. O nome, localização e o conteúdo desse arquivo mudam de acordo com o módulo. **É necessário que você também edite esse arquivo e altere os valores definidos como **CHANGE_HERE** para os valores condizentes com seu ambiente híbrido.**

* As **linhas 6 e 7** contém uma referência para as **linhas 16 a 18**, que definem a rede Docker a ser usada pelo contêiner que executará esse módulo da plataforma. Você não precisa alterar o conteúdo dessa seção.

* A **linha 8** contém o endereço do Docker Registry (``gcr.io/production-main-231423``), imagem Docker do módulo da plataforma (``api-gateway``) e versão do módulo (``4.2.0.0``). **Você deve entrar em contato com o time da Sensedia para saber qual a URL do Docker Registry, nome da imagem docker do módulo e a versão que deve utilizar e alterar no arquivo ``*.yaml`` antes de fazer o deploy.**

* A **linha 9** contém o nome do contêiner que executará o módulo da plataforma. Você não precisa alterar.

* Na **linha 10** você pode definir quantas CPUs que o serviço executado no contêiner pode utilizar. Nem todos os módulos contém essa definição por padrão.

* Na **linha 11** você pode definir o limite de memória RAM que o serviço executado no contêiner pode utilizar. Nem todos os módulos contém essa definição por padrão.

* Na **linha 12** é definida a política de restart do contêiner. **É recomendado manter o valor ``always``**, para que o contêiner seja reiniciado automaticamente em caso de problemas, evitando a necessidade de uma intervenção manual.

* A **linha 13** contém um palavra reservada da sintaxe nativa do Docker Compose, que indica o início de uma seção para definição das portas (por padrão TCP-*Transmission Control Protocol*) que serão utilizadas pelo contêiner.

* A **linha 14** contém a porta a ser utilizada pelo host e pelo contêiner para permitir o acesso externo ao módulo da plataforma. As portas são definidas no seguinte padrão:

PORTA_HOST:PORTA_contêiner

Exemplo 2: ``- '8080:8080'`` - a porta do host é 8080/TCP, que escutará as requisições e encaminhará para a porta do contêiner, que é 8080/TCP.

**A porta do host pode ser alterada** conforme a necessidade do seu ambiente, mas a porta do contêiner **não deve ser alterada**.

Exemplo 3: ``- '80:8080'``. Neste caso, a porta do host é 80/TCP e a a porta do contêiner é 8080/TCP.

    ATENÇÃO!!! A porta do contêiner não é acessível fora do host. Ela só é acessível dentro do host.

    Todas as requisições são ouvidas na porta do host e encaminhadas para a porta do contêiner, de forma transparente ao usuário.

    Ao configurar regras de firewall, leve em consideração apenas as portas do host.

    A porta do host sempre fica à esquerda do ``:``.

    A porta do contêiner sempre fica à direita do ``:``.

    Por padrão, todas as portas são TCP.

    A porta do host pode ser alterada, mas deve ser uma porta diferente para cada contêiner a ser iniciado.

    A porta do contêiner não pode ser alterada e é definida pelo time da Sensedia no momento em que a imagem Docker está sendo construída.

    Pode haver uma ou mais portas de host e contêiner para cada módulo.

### Criação de Token

A configuração do ambiente híbrido tem como pré-requisito a utilização de um token da plataforma. O token deve ser criado utilizando o seguinte procedimento:

1. Acesse o API Manager;
2. Clique no menu da página de **Access Token**;
3. Clique no botão **Criar Access token**;
4. O campo **Owner** deve conter o email de um usuário responsável pelo ambiente;
5. Defina o valor **API-Platform Integration** no campo **APP Field**;
6. Na próxima página, selecione a API **API Manager 3.0.0**;
7. Selecione o Plano **Federated Plan**;
8. Publique o token por meio do botão **Publish your access token**;
9. Guarde o token gerado (ele será utilizado no provisionamento dos módulos).

Quando o método de instalação for **Poc-all-in-one**:

Edite o arquivo ``poc-all-in-one/hybrid.env``, localize os seguintes parâmetros e substitua o termo ``CHANGE_HERE`` pelo token gerado anteriormente.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE

Quando o método de instalação for **All-In-One**:

Edite o arquivo ``all-in-one/hybrid.env``, localize os seguintes parâmetros e substitua o termo ``CHANGE_HERE`` pelo token gerado anteriormente.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE

Quando o método de instalação for **Modules**:

Edite os arquivos ``modules/logstash-federated/logstash.env``, ``modules/agent-gateway/agent-gateway.env`` e ``modules/agent-authorization/agent-authorization.env``, localize os seguintes parâmetros e substitua o termo ``CHANGE_HERE`` pelo token gerado anteriormente.

* WEBSOCKET_SENSEDIAAUTH=CHANGE_HERE
* SENSEDIA_APIPLATFORM_FEDERATED_ACCESSTOKEN=CHANGE_HERE
