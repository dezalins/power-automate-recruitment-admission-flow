# 🗺️ Mapeamento de Campos entre Listas

Este documento descreve a correspondência de campos entre a lista de **Recrutamento** (origem) e a lista de **Cargos e Documentação** (destino).

---

## Tabela de Mapeamento

| Campo Destino (Cargos Documentação) | Campo Origem (Recrutamento) | Descrição |
|---|---|---|
| `Title` | `field_13` | Número da Requisição |
| `field_2` | `{Name}` | Nome do Candidato |
| `Empresa_x002f_Filial` | `Empresa_x002f_Filial/Value` | Empresa / Filial |
| `field_6` | `Sal_x00e1_rio` | Salário |
| `field_3` | `field_15` | Função / Cargo |
| `field_4` | `Datadeadmiss_x00e3_o` | Data de Admissão |
| `field_17` | `IDdogestor` | ID do Gestor |
| `field_18` | `Nomedogestor` | Nome do Gestor |
| `field_20` | `Descri_x00e7__x00e3_odohor_x00e1` | Descrição do Horário |
| `field_13` | `Descri_x00e7__x00e3_odase_x00e7_` | Descrição da Seção |
| `field_19` | `Subuniorg` | Sub-unidade Organizacional |
| `field_22` | `Cargahor_x00e1_riamensal_x002f_s/Value` | Carga Horária Mensal/Semanal |
| `field_23` | `Contratohibrido_x003f_/Value` | Contrato Híbrido? |
| `field_15` | `PCD/Value` | PCD (Pessoa com Deficiência) |
| `field_16` | `G_x00ea_nero` | Gênero |
| `field_10` | `field_27` | Campo complementar 1 |
| `field_11` | `field_28` | Campo complementar 2 |
| `field_12` | `field_29` | Campo complementar 3 |
| `field_8` | `field_30` | Campo complementar 4 |
| `field_14` | `field_31` | Campo complementar 5 |
| `field_9` | `field_32` | Campo complementar 6 |
| `field_7` | `field_33` | Campo complementar 7 |
| `N_x00b0_daresid_x00ea_ncia` | `N_x00fa_merodaresid_x00ea_ncia` | Número da Residência |
| `field_21` | `Modified` | Data da última modificação |
| `field_5` | `Solicitadopor_x003a_/Value` | Solicitado por |
| `field_0` | `Empresa_x002f_Filial/Value` | Empresa / Filial (redundante) |
| `idoriginal` | `ID` | ID original (chave de vínculo entre listas) |
| `Status/Value` | *(fixo)* `"Iniciado"` | Status inicial do processo CSC |

---

## Campos de Atualização vs. Criação

Alguns campos diferem entre o momento de **criação** e o de **atualização** de um item:

| Campo Destino | Criação | Atualização |
|---|---|---|
| `field_21` | `Modified` (data da modificação) | `Datadasolicita_x00e7__x00e3_o` (data original da solicitação) |

> **Observação:** No cenário de atualização, o campo `field_21` recebe a data da solicitação original, preservando o histórico. Na criação, recebe a data de modificação do item na lista de origem.

---

## Chave de Relacionamento entre Listas

O campo **`idoriginal`** é o elo de relacionamento entre as duas listas. Ele armazena o `ID` do item na lista de Recrutamento, permitindo que o fluxo:

1. Verifique se já existe um item correspondente na lista de destino (filtro: `idoriginal eq '@{triggerBody()?['ID']}'`)
2. Decida entre **criar** (novo registro) ou **atualizar** (registro existente)

---

## Migração de Anexos

Os anexos são migrados individualmente para o item correspondente na lista de destino:

| Propriedade | Origem | Destino |
|---|---|---|
| Nome do arquivo | `DisplayName` do anexo original | `displayName` no novo item |
| Conteúdo | `GetAttachmentContent` da lista Recrutamento | `CreateAttachment` na lista Cargos Documentação |
| Item de destino | — | Primeiro item retornado pela busca por `idoriginal` |
