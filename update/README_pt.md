# Sensedia API Platform Hybrid - Guia de Atualização de Módulos

Este repositório contém os arquivos de configuração para a implantação e atualização dos módulos da **Sensedia API Platform Hybrid**. Siga as instruções abaixo para garantir uma atualização segura e homologada pelo nosso time de engenharia.

---

## 📌 Fluxo de Atualização

Para garantir o sucesso e a estabilidade do seu ambiente, o processo de atualização segue duas etapas obrigatórias:

### 1. Abertura de Ticket (Suporte)
Antes de realizar qualquer alteração, é nescessario **abrir um ticket de suporte** solicitando a atualização. 
* *Nota: O alinhamento prévio é essencial para que nossa equipe acompanhe o processo, se necessário.*

### 2. Alinhamento de Data e Envio das Versões
Após o alinhamento da data ideal para a janela de manutenção, o time de engenharia da Sensedia enviará formalmente os **dados e as tags da nova versão** a ser aplicada.

---

## 🛠️ Como Atualizar os Módulos

Dependendo da arquitetura utilizada no seu ambiente (**Kubernetes** ou **Docker Compose**), siga o guia correspondente abaixo. Você precisará alterar a versão/tag de cada módulo específico recebido no ticket.

⏩ A atualização dos moduos devem ser realizadas na seguinte ordem:

🔻Logstash-Federated

🔻Agent-Gateway

🔻Agent-Authorization

🔻Api-Authorization

🔻Api-Gateway

### ☸️ Cenário 1: Kubernetes (Helm)
Se o seu ambiente roda em Kubernetes, os exemplos de configuração dos módulos estão localizados na pasta:
👉 [`kubernetes/helm/values_examples`](https://github.com/Sensedia/api-platform-hybrid/tree/PLATS-199/kubernetes/helm/values_examples)

Para atualizar, acesse o arquivo `values.yaml` correspondente ao módulo desejado e altere o campo de `tag` ou `version` para a versão homologada enviada pela Sensedia.

⏩ Realize a alteração da versão no campo tag 
```bash  
tag: "CHANGE_HERE"
```
⏩ Realize o update de cada modulo

Altere o comando abaixo para cada modulo correspondente com o nome de cada módulo, siga o exemplo abaixo:
```bash
helm upgrade agent-gateway sensedia-helm-s3/agent-gateway --namespace MY_HYBRID_ENV --values ~/api-platform-hybrid/agent-gateway.yaml

```

### 🐳 Cenário 2: Docker Compose
Se o seu ambiente roda utilizando Docker Compose, as definições de cada módulo estão localizadas na pasta:
👉 [`compose/modules`](https://github.com/Sensedia/api-platform-hybrid/tree/PLATS-199/compose/modules)

Para atualizar, acesse o arquivo `Nome_do_modulo.yaml` correspondente ao módulo desejado e altere o campo de `image` para a versão homologada enviada pela Sensedia.

⏩ Realize a alteração da versão: 
```bash  
    image: gcr.io/production-main-268117/agent-gateway:#CHANGE_HERE
```
⏩ Realize o update de cada modulo

Altere o comando abaixo para cada modulo correspondente com o nome de cada módulo, siga o exemplo abaixo:
```bash
sudo docker-compose -f logstash-federated.yaml up -d

```

> ⚠️ **Atenção:** Nunca aplique versões que não foram explicitamente homologadas e enviadas pelo time de suporte da Sensedia no seu ticket de atendimento.

---
💡 *Dúvidas ou problemas durante o processo? Entre em contato através do seu canal oficial de suporte da Sensedia.*
