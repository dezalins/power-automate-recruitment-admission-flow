# 📋 Migração entre Listas — Recrutamento e Admissão

[![Power Automate](https://img.shields.io/badge/Automation-Power%20Automate-0066FF?style=for-the-badge&logo=powerautomate&logoColor=white)](https://flow.microsoft.com/)
[![SharePoint](https://img.shields.io/badge/Platform-SharePoint-0078D4?style=for-the-badge&logo=microsoftsharepoint&logoColor=white)](https://www.microsoft.com/microsoft-365/sharepoint/collaboration)
[![Outlook](https://img.shields.io/badge/Communication-Outlook-0072C6?style=for-the-badge&logo=microsoftoutlook&logoColor=white)](https://www.microsoft.com/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook)
[![Microsoft 365](https://img.shields.io/badge/Suite-Microsoft%20365-D83B01?style=for-the-badge&logo=microsoftoffice&logoColor=white)](https://www.microsoft.com/microsoft-365)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)](#)

> **Power Automate Flow** para automação do processo de admissão de colaboradores no portal CSC de grande empresa no Brasil.

---

## 📌 Visão Geral

Este fluxo automatiza a **migração de dados** entre listas do SharePoint no processo de recrutamento e admissão. Quando um item é criado ou atualizado na lista de **Recrutamento**, o fluxo verifica se a solicitação está aprovada, valida a documentação obrigatória e replica os dados para a lista de **Cargos e Documentação**, notificando as equipes envolvidas por e-mail.

| Atributo | Valor |
|---|---|
| **Nome do Flow** | Migração entre listas - Recrutamento e admissão |
| **Plataforma** | Microsoft Power Automate |
| **Ambiente** | SharePoint Online + Office 365 Outlook |
| **Frequência de execução** | A cada 1 minuto |
| **Criado em** | 30/04/2026 |
| **Última modificação** | 29/04/2026 |

---

## 🏗️ Arquitetura

```
Lista: Recrutamento (SharePoint)
        │
        ▼ [Trigger: Item criado ou modificado]
        │
        ├── Condição: Status = "Aprovado"?
        │       │
        │       ├── [SIM] Condição: Cargo é Motorista/Aprendiz ou vazio?
        │       │           │
        │       │           ├── [SIM - Cargo Especial] Verifica anexos
        │       │           │       ├── Sem anexos → E-mail solicitante (pendência docs)
        │       │           │       └── Com anexos → Cria ou Atualiza item na lista Cargos Documentação
        │       │           │                          └── Notifica CSC + Solicitante por e-mail
        │       │           │                          └── Migra anexos para novo item
        │       │           │
        │       │           └── [NÃO - Outros Cargos] Mesma lógica acima
        │       │
        │       └── [NÃO] Nenhuma ação
        │
        └── For Each: Itens existentes em Cargos Documentação
                └── Condição: Cargo é Operador/Manuseador?
                        └── [SIM] Cria item adicional + Notifica CSC + Solicitante
```

---

## ⚙️ Conectores Utilizados

| Conector | Finalidade |
|---|---|
| **SharePoint Online** | Leitura e escrita nas listas de Recrutamento e Cargos/Documentação |
| **Office 365 Outlook** | Envio de notificações por e-mail |

---

## 🗂️ Listas do SharePoint Envolvidas

### Lista de Origem — Recrutamento
- **Site:** `https://company.sharepoint.com/teams/PeopleAnalyticsBR`
- **ID da Lista:** `(Não incluído, informação confidencial)`
- **Papel:** Fonte dos dados do processo de recrutamento e admissão

### Lista de Destino — Cargos e Documentação
- **Site:** `https://company.sharepoint.com/teams/PeopleAnalyticsBR`
- **ID da Lista:** `(Não incluído, informação confidencial)`
- **Papel:** Destino dos dados migrados para o processo de documentação CSC

---

## 🔄 Lógica do Fluxo Detalhada

Consulte o arquivo [`docs/logica-detalhada.md`](docs/logica-detalhada.md) para a descrição completa de cada etapa do fluxo.

---

## 📧 Notificações por E-mail

O fluxo envia notificações automáticas em diferentes cenários. Veja [`docs/notificacoes.md`](docs/notificacoes.md) para detalhes dos templates e destinatários.

---

## 🗺️ Mapeamento de Campos

A correspondência entre campos das duas listas está documentada em [`docs/mapeamento-campos.md`](docs/mapeamento-campos.md).

---

## 🚀 Como Importar o Flow

Consulte [`docs/importacao.md`](docs/importacao.md) para o passo a passo de importação e configuração do fluxo em um novo ambiente.

---

## 📁 Estrutura do Repositório

```
├── README.md                        # Este arquivo
├── Migraçãodelistas_*.zip           # Pacote exportado do Power Automate
├── docs/
│   ├── logica-detalhada.md          # Descrição passo a passo do fluxo
│   ├── mapeamento-campos.md         # De/Para dos campos entre listas
│   ├── notificacoes.md              # Templates e destinatários dos e-mails
│   └── importacao.md                # Guia de importação e configuração
└── .github/
    └── PULL_REQUEST_TEMPLATE.md     # Template para Pull Requests
```

---

## 👥 Equipe CSC — Destinatários de Notificação

| E-mail | Papel |
|---|---|
| `funcionario1@company.com` | Time CSC |
| `funcionario2@company.com` | Time CSC |
| `funcionario3@company.com` | Time CSC |

> **Remetente:** `PeopleAnalyticsBR@company.onmicrosoft.com`

---

## ⚠️ Observações Importantes

- O fluxo roda **a cada minuto** — garanta que as conexões estejam sempre autenticadas.
- A conta de serviço `andreza.lins@company.com` é a proprietária das conexões SharePoint e Outlook.
- O status inicial criado na lista de destino é sempre **"Iniciado"**.
- Anexos são **migrados automaticamente** da lista de origem para a lista de destino.

---

## 🔗 Links Úteis

- Portal SharePoint PeopleAnalyticsBR (Não incluído, informação confidencial)
- [Documentação Power Automate](https://learn.microsoft.com/pt-br/power-automate/)
