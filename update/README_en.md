# Sensedia API Platform Hybrid - Module Update Guide

This repository contains the configuration files required to deploy and update the **Sensedia API Platform Hybrid** modules. Follow the instructions below to ensure a safe update process validated by our engineering team.

---

## 📌 Update Workflow

To ensure the success and stability of your environment, the update process consists of two mandatory steps:

### 1. Open a Support Ticket
Before making any changes, you must **open a support ticket** requesting the update.

> **Note:** Prior coordination is essential so our engineering team can monitor the process if necessary.

### 2. Schedule the Maintenance Window and Receive the New Version
After the maintenance window has been agreed upon, the Sensedia Engineering team will officially provide the **version information and image tags** to be applied.

---

## 🛠️ How to Update the Modules

Depending on your environment architecture (**Kubernetes** or **Docker Compose**), follow the appropriate guide below. You will need to update the version/tag of each specific module provided in your support ticket.

⏩ The modules **must be updated in the following order**:

🔻Logstash-Federated

🔻Agent-Gateway

🔻Agent-Authorization

🔻API-Authorization

🔻API-Gateway

### ☸️ Scenario 1: Kubernetes (Helm)

If your environment is running on Kubernetes, the module configuration examples are located in the following directory:

👉 [`kubernetes/helm/values_examples`](https://github.com/Sensedia/api-platform-hybrid/tree/PLATS-199/kubernetes/helm/values_examples)

To update a module, open the corresponding `values.yaml` file and change the `tag` (or `version`, depending on the module) to the version provided and approved by Sensedia.

⏩ Update the version in the `tag` field:

```bash
tag: "CHANGE_HERE"
```

⏩ Upgrade each module

Replace the module name in the command below with the appropriate one, as shown in the example:

```bash
helm upgrade agent-gateway sensedia-helm-s3/agent-gateway --namespace MY_HYBRID_ENV --values ~/api-platform-hybrid/agent-gateway.yaml
```

### 🐳 Scenario 2: Docker Compose

If your environment is running with Docker Compose, the module definitions are located in:

👉 [`compose/modules`](https://github.com/Sensedia/api-platform-hybrid/tree/PLATS-199/compose/modules)

To update a module, open the corresponding `module_name.yaml` file and change the `image` field to the version provided and approved by Sensedia.

⏩ Update the image version:

```bash
image: gcr.io/production-main-268117/agent-gateway:#CHANGE_HERE
```

⏩ Update each module

Replace the filename in the command below with the corresponding module file, as shown in the example:

```bash
sudo docker-compose -f logstash-federated.yaml up -d
```

> ⚠️ **Warning:** Never deploy versions that have not been explicitly approved and provided by the Sensedia Support team through your support ticket.

---

💡 *If you have any questions or encounter issues during the update process, please contact Sensedia through your official support channel.*
