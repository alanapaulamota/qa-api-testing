# Bug Reports

## 1. Identificação

**Projeto:** QA API Testing  
**Sistema sob teste:** Restful Booker API  
**Ferramenta:** Postman  
**Versão:** 1.0

---

## 2. Critérios para registro

Um comportamento observado durante a execução dos testes somente será registrado como defeito quando houver evidência suficiente de que:

- o comportamento observado diverge do comportamento esperado;
- o cenário é reproduzível;
- o comportamento não pode ser explicado por configuração, autenticação, dados de teste ou características conhecidas do ambiente;
- existe impacto funcional identificável.

Ocorrências relacionadas ao ambiente compartilhado, expiração de token ou inconsistências de execução serão registradas como observações quando não houver evidência suficiente para classificá-las como defeito.

---

## 3. Defeitos identificados

Até o momento, nenhum defeito foi confirmado.

---

## 4. Observações de execução

### OBS-001 — Instabilidade de dados no ambiente compartilhado

**Tipo:** Observação de ambiente  
**Severidade:** N/A  
**Status:** Observado

Durante diferentes execuções, alguns identificadores de reservas deixaram de estar disponíveis posteriormente ou apresentaram dados diferentes dos esperados.

Esse comportamento foi observado durante a utilização do ambiente público e compartilhado da Restful Booker API.

**Impacto:**

Pode dificultar a reprodução de cenários que dependem de um `bookingId` previamente utilizado.

**Tratamento durante os testes:**

Foram utilizados novos dados de teste e novos identificadores quando necessário, priorizando a execução imediata do fluxo.

---

### OBS-002 — Respostas relacionadas à autenticação durante a execução

**Tipo:** Observação de execução  
**Status:** Observado

Durante algumas operações que exigiam autenticação, foram recebidas respostas `403 Forbidden`.

Após a renovação do token e a verificação da configuração da requisição, algumas operações puderam ser executadas normalmente.

**Tratamento durante os testes:**

O token foi renovado e a requisição foi executada novamente.

**Classificação:**

Não foi registrado como defeito da API, pois não houve evidência suficiente para determinar que o comportamento representava uma falha funcional do sistema.

---

### OBS-003 — Ocorrência de 405 durante tentativa de PATCH

**Tipo:** Observação de execução  
**Status:** Observado

Durante uma tentativa de execução do endpoint de atualização parcial, foi recebida uma resposta `405 Method Not Allowed`.

A requisição foi posteriormente revisada e o cliente Postman foi reiniciado. Em nova execução, utilizando um registro válido, o método `PATCH` foi processado com sucesso.

**Classificação:**

Não foi registrado como defeito da API, pois o comportamento não foi reproduzido de forma consistente após a revisão da requisição e nova execução.
