# 📧 Notificações por E-mail

O fluxo envia e-mails automáticos em diferentes cenários do processo de admissão.

**Remetente padrão:** `PeopleAnalyticsBR@company.onmicrosoft.com`

---

## 1. E-mail de Documentação Pendente (Solicitante)

**Quando é enviado:** Item aprovado, mas **sem anexos** na lista de Recrutamento.  
**Destinatário:** Solicitante (`Solicitadopor_x003a_/Value`)  
**Assunto:** `Requisição de Admissão - {Nº Requisição}`

**Conteúdo:**
> Prezado(a) {Nome do Editor},
>
> Você incluiu a requisição de nro {Requisição} na Lista de Recrutamento e seleção.
>
> Lembre-se: A solicitação de admissão somente é processada após anexar a documentação necessária para o cargo de {Função}.
>
> Link para o item: {Link}
>
> Atenciosamente,  
> Time CSC.

---

## 2. E-mail de Nova Admissão para o CSC

**Quando é enviado:** Item aprovado com anexos — **novo item criado** na lista de Cargos Documentação.  
**Destinatários:**
- `funcionario1@company.com`
- `funcionario2@company.com`
- `funcionario3@company.com`

**Assunto:** `Admissão de: {Nome do Candidato}`

**Conteúdo:**
> Prezado(a),
>
> Uma nova solicitação de admissão foi inserida no portal CSC:
>
> Nome: {Nome}  
> Requisição: {Nº Requisição}  
> Função: {Função}  
> Descrição do Horário: {Descrição Horário}  
> Carga Horária: {Carga Horária}  
> Contrato Híbrido: {Contrato Híbrido}  
> Data de Admissão: {Data Admissão}  
> Empresa: {Empresa/Filial}  
> Foi criada por: {Solicitado por}
>
> Atenciosamente,  
> Time CSC.

---

## 3. E-mail de Nova Admissão para o Solicitante

**Quando é enviado:** Item aprovado com anexos — **novo item criado** (logo após o e-mail ao CSC).  
**Destinatário:** Solicitante (`Solicitadopor_x003a_/Value`)  
**Assunto:** `Admissão de: {Nome do Candidato}`

**Conteúdo:**
> Prezado(a),
>
> Uma nova solicitação de admissão foi inserida no portal CSC:
>
> Requisição: {Nº Requisição}  
> Nome: {Nome}  
> Função: {Função}  
> Descrição do Horário: {Descrição Horário}  
> Carga Horária: {Carga Horária}  
> Contrato Híbrido: {Contrato Híbrido}  
> Data de Admissão: {Data Admissão}  
> Empresa: {Empresa/Filial}
>
> *Mensagem automática, favor não responder.

---

## 4. E-mail de Atualização de Admissão para o CSC

**Quando é enviado:** Item aprovado com anexos — **item existente atualizado** na lista de Cargos Documentação.  
**Destinatários:** Mesmo grupo do CSC (itens 2 acima)  
**Assunto:** `Admissão de: {Nome do Candidato}`

**Conteúdo:**
> Prezado(a),
>
> A solicitação de admissão foi **atualizada** no portal CSC:
>
> Nome: {Nome}  
> Requisição: {Nº Requisição}  
> Função: {Função}  
> Descrição do Horário: {Descrição Horário}  
> Carga Horária: {Carga Horária}  
> Contrato Híbrido: {Contrato Híbrido}  
> Data de Admissão: {Data Admissão}  
> Empresa: {Empresa/Filial}  
> Foi criada por: {Solicitado por}
>
> Atenciosamente,  
> Time CSC.

---

## Resumo dos Cenários de Notificação

| Cenário | Destinatário CSC | Destinatário Solicitante |
|---|:---:|:---:|
| Sem documentação anexada | ❌ | ✅ (alerta pendência) |
| Nova admissão criada | ✅ | ✅ (confirmação) |
| Admissão atualizada | ✅ | ❌ |

---

## Sensibilidade dos E-mails

Alguns e-mails para o CSC são enviados com configuração de sensibilidade específica (campo `Sensitivity`), indicando que se trata de comunicação interna confidencial. Os e-mails ao solicitante são enviados com importância `Normal` sem sensibilidade especial.
