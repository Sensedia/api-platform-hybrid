# Sensedia API Platform Hybrid - Guía de Actualización de Módulos

Este repositorio contiene los archivos de configuración para la implantación y actualización de los módulos de la **Sensedia API Platform Hybrid**. Siga las instrucciones a continuación para garantizar una actualización segura y homologada por nuestro equipo de ingeniería.

---

## 📌 Flujo de Actualización

Para garantizar el éxito y la estabilidad de su entorno, el proceso de actualización sigue dos etapas obligatorias:

### 1. Apertura de Ticket (Soporte)
Antes de realizar cualquier cambio, es necesario **abrir un ticket de soporte** solicitando la actualización. 
* *Nota: La alineación previa es esencial para que nuestro equipo pueda acompañar el proceso, si fuera necesario.*

### 2. Alineación de Fecha y Envío de Versiones
Tras acordar la fecha ideal para la ventana de mantenimiento, el equipo de ingeniería de Sensedia enviará formalmente los **datos y las etiquetas (tags) de la nueva versión** que se aplicará.

---

## 🛠️ Cómo Actualizar los Módulos

Dependiendo de la arquitectura utilizada en su entorno (**Kubernetes** o **Docker Compose**), siga la guía correspondiente a continuación. Deberá cambiar la versión/tag de cada módulo específico recibido en el ticket.

⏩ La actualización de los módulos debe realizarse en el siguiente orden:

🔻Logstash-Federated

🔻Agent-Gateway

🔻Agent-Authorization

🔻Api-Authorization

🔻Api-Gateway

### ☸️ Escenario 1: Kubernetes (Helm)
Si su entorno se ejecuta en Kubernetes, los ejemplos de configuración de los módulos se encuentran en la carpeta:
👉 [`kubernetes/helm/values_examples`](https://github.com/Sensedia/api-platform-hybrid/tree/PLATS-199/kubernetes/helm/values_examples)

Para actualizar, acceda al archivo `values.yaml` correspondiente al módulo deseado y cambie el campo `tag` o `version` por la versión homologada enviada por Sensedia.

⏩ Realice el cambio de versión en el campo tag:
```bash  
tag: "CHANGE_HERE"
```
⏩ Realice la actualización (update) de cada módulo

Modifique el comando siguiente para cada módulo correspondiente con el nombre de cada uno, siga el ejemplo a continuación:
```bash
helm upgrade agent-gateway sensedia-helm-s3/agent-gateway --namespace MY_HYBRID_ENV --values ~/api-platform-hybrid/agent-gateway.yaml
```

🐳 Escenario 2: Docker Compose
Si su entorno se ejecuta utilizando Docker Compose, las definiciones de cada módulo se encuentran en la carpeta:
👉 compose/modules

Para actualizar, acceda al archivo Nombre_del_modulo.yaml correspondiente al módulo deseado y cambie el campo image por la versión homologada enviada por Sensedia.

⏩ Realice el cambio de versión:
```bash  
    image: gcr.io/production-main-268117/agent-gateway:#CHANGE_HERE
```
⏩ Realice la actualización (update) de cada módulo

Modifique el comando siguiente para cada módulo correspondiente con el nombre de cada uno, siga el ejemplo a continuación:
```bash
sudo docker-compose -f logstash-federated.yaml up -d

```

> ⚠️ Atención: Nunca aplique versiones que no hayan sido explícitamente homologadas y enviadas por el equipo de soporte de Sensedia en su ticket de atención.

---
💡 ¿Dudas o problemas durante el proceso? Póngase en contacto a través de su canal oficial de soporte de Sensedia.
