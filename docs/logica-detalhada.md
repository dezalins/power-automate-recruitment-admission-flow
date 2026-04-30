# 🔍 Lógica Detalhada do Fluxo

## Trigger — Gatilho de Execução

**Tipo:** `GetOnUpdatedItems` (SharePoint — Item criado ou modificado)  
**Frequência:** A cada 1 minuto  
**Lista monitorada:** Recrutamento (`Confidencial`)

O fluxo é disparado para **cada item individualmente** (`splitOn: @triggerOutputs()?['body/value']`), ou seja, se vários itens forem modificados ao mesmo tempo, o fluxo executa em paralelo para cada um.

---

## Bloco 1 — Condição: Verificar se está Aprovado

```
SE Aprovação/Value == "Aprovado"
    → Entra no bloco principal
SENÃO
    → Nenhuma ação (fluxo encerra)
```

Esta é a porta de entrada principal. Apenas solicitações com status **"Aprovado"** avançam no processo.

---

## Bloco 2 — Condição: Cargo (Categorização por tipo de vaga)

O fluxo categoriza o cargo em dois grupos com base no campo `field_15` (Função):

### Grupo A — Cargos Especiais (com verificação de documentação obrigatória)
Inclui vagas que contêm (case-insensitive): `Motorista` ou `Aprendiz`.

### Grupo B — Outros Cargos
Todos os demais cargos não enquadrados no Grupo A.

---

## Bloco 3 — Verificação de Anexos

Para **ambos os grupos**, o fluxo verifica se o item da lista de Recrutamento possui anexos.

```
Ação: Obter_anexos (GetItemAttachments)
    └── Lista: Recrutamento
    └── Item: ID do item disparador

SE quantidade de anexos > 1
    → Prossegue para migração
SENÃO
    → Envia e-mail ao solicitante alertando sobre documentação pendente
    → Fluxo encerra para este item
```

> **Nota:** A condição verifica `> 1` para o Grupo A e `> 0` para o Grupo B.

---

## Bloco 4 — Criar ou Atualizar Item na Lista de Destino

Após confirmar a existência de anexos, o fluxo verifica se já existe um item correspondente na lista de Cargos e Documentação (filtro por `idoriginal == ID do item de origem`).

### Cenário: Item ainda NÃO existe (novo registro)

**Ações em sequência:**
1. **Criar item** na lista Cargos Documentação com todos os campos mapeados e status `"Iniciado"`
2. **Enviar e-mail ao CSC** notificando nova admissão
3. **Enviar e-mail ao solicitante** confirmando o recebimento
4. **Buscar ID do novo item** criado na lista de destino
5. **Obter lista de anexos** do item de origem
6. **Para cada anexo:**
   - Obter conteúdo do arquivo (`GetAttachmentContent`)
   - Adicionar na lista de destino (`CreateAttachment`)

### Cenário: Item JÁ existe (atualização)

**Ações em sequência:**
1. **Atualizar item** existente com os novos dados do campo de origem
2. **Enviar e-mail ao CSC** notificando atualização da admissão
3. **Buscar o item atualizado** na lista de destino
4. **Obter lista de anexos** do item de origem
5. **Para cada anexo:**
   - Obter conteúdo
   - Adicionar na lista de destino (mesmo processo de criação)

---

## Bloco 5 — For Each: Verificação por Cargo de Operador/Manuseador

Após o bloco principal, o fluxo percorre todos os itens já existentes na lista de Cargos Documentação e verifica se o cargo se enquadra em:

- `Operador de serviço logístico` (qualquer capitalização)
- `Operador de carga` (qualquer capitalização)
- `Manuseador de carga` (qualquer capitalização)

Se sim, cria um novo item adicional para esse cargo na lista de destino, além de enviar as notificações de e-mail correspondentes.

---

## Diagrama de Sequência Simplificado

```
[Item modificado na lista Recrutamento]
            │
            ▼
    [Está Aprovado?] ──NÃO──> [FIM]
            │
           SIM
            │
            ▼
  [Cargo: Motorista/Aprendiz?]
     │                    │
    SIM                  NÃO
     │                    │
     ▼                    ▼
[Obter Anexos]      [Obter Anexos]
     │                    │
  [> 1 anexo?]        [> 0 anexos?]
   │       │            │       │
  NÃO     SIM          NÃO     SIM
   │       │            │       │
   ▼       ▼            ▼       ▼
[E-mail  [Buscar   [E-mail  [Buscar
pendência] item]   pendência] item]
            │                  │
    [Existe na lista destino?] │
       │             │         │
      SIM            NÃO      ...
       │              │
  [Atualizar]    [Criar item]
       │              │
  [Notificar]    [Notificar]
       │              │
  [Migrar anexos] [Migrar anexos]
```
