# Test Report — Restful Booker API

## 1. Identificação

**Projeto:** QA API Testing  
**Sistema sob teste:** Restful Booker API  
**Ferramenta:** Postman  
**Versão:** 1.0  
**Período de execução:** Outubro de 2026  
**Status:** Em execução

---

## 2. Objetivo

Este relatório apresenta os resultados obtidos durante a execução dos testes da Restful Booker API.

Os testes tiveram como objetivo verificar o comportamento dos principais recursos relacionados ao ciclo de vida de uma reserva, incluindo:

- autenticação;
- consulta;
- criação;
- atualização completa;
- atualização parcial;
- exclusão;
- fluxo integrado.

Também foram realizadas validações sobre:

- códigos de status HTTP;
- formato das respostas;
- estrutura dos objetos;
- tipos de dados;
- conteúdo retornado;
- persistência das alterações;
- comportamento após exclusão.

---

## 3. Escopo executado

| Funcionalidade       | Método | Endpoint               |
| -------------------- | ------ | ---------------------- |
| Autenticação         | POST   | `/auth`                |
| Listagem             | GET    | `/booking`             |
| Consulta             | GET    | `/booking/{bookingId}` |
| Criação              | POST   | `/booking`             |
| Atualização completa | PUT    | `/booking/{bookingId}` |
| Atualização parcial  | PATCH  | `/booking/{bookingId}` |
| Exclusão             | DELETE | `/booking/{bookingId}` |

---

## 4. Estratégia

Foram utilizados testes positivos e negativos.

Os testes positivos verificaram operações com dados válidos.

Os testes negativos foram planejados para avaliar comportamentos como:

- credenciais inválidas;
- ausência de credenciais;
- identificadores inexistentes;
- dados incompletos;
- tipos inválidos;
- ausência de autenticação;
- token inválido.

Os testes executados no Postman validaram automaticamente respostas e dados relevantes.

---

## 5. Resultados

### 5.1 Casos executados

| Caso               | Resultado            |
| ------------------ | -------------------- |
| TC-AUTH-001        | Pass                 |
| TC-BOOK-LIST-001   | Pass                 |
| TC-BOOK-GET-001    | Pass                 |
| TC-BOOK-GET-002    | Pass                 |
| TC-BOOK-CREATE-001 | Pass                 |
| TC-BOOK-PUT-001    | Pass                 |
| TC-BOOK-PATCH-001  | Pass                 |
| TC-BOOK-DELETE-001 | Pass                 |
| TC-E2E-001         | Pass — fluxo parcial |

### 5.2 Resumo

**Casos executados:** 9  
**Pass:** 9  
**Fail:** 0  
**Bloqueados:** 0  
**Defeitos confirmados:** 0

> O resultado do E2E representa o fluxo parcial efetivamente executado e não deve ser interpretado como execução de todas as etapas originalmente planejadas.

---

## 6. Principais validações

### Autenticação

Foi validada a geração de token utilizando credenciais válidas.

O token retornado foi armazenado automaticamente em variável de ambiente do Postman.

### Listagem

Foi validado que o endpoint:

- retorna `200 OK`;
- retorna JSON;
- retorna uma lista;
- apresenta identificadores de reserva.

### Consulta

Foram validados:

- status HTTP;
- formato JSON;
- campos principais;
- tipos de dados;
- estrutura das datas.

Também foi validado o comportamento após exclusão, com retorno `404 Not Found`.

### Criação

Foi validada a criação de uma reserva com dados válidos.

Foram verificadas:

- resposta HTTP;
- formato JSON;
- `bookingid`;
- dados da reserva;
- correspondência dos dados enviados e retornados.

### Atualização completa

Foi validada a atualização completa utilizando `PUT`.

Os principais campos e datas foram validados após a operação.

### Atualização parcial

Foi validada a alteração de um único campo utilizando `PATCH`.

Os demais campos foram verificados para confirmar que permaneceram inalterados.

### Exclusão

Foi validada a exclusão de uma reserva.

A API retornou:

```text
201 Created
```

Após a exclusão, uma nova consulta retornou:

```text
404 Not Found
```

---

## 7. Fluxo End-to-End

Foi executado um fluxo integrado utilizando uma reserva criada durante a própria execução.

```text
Create
  ↓
Get
  ↓
PATCH
  ↓
Delete
  ↓
Get
```

O fluxo utilizou o identificador `2937`.

### Resultados

| Etapa           | Resultado              |
| --------------- | ---------------------- |
| Create          | Pass                   |
| Get             | Pass                   |
| PATCH           | Pass                   |
| Delete          | Pass                   |
| Get após Delete | Pass — `404 Not Found` |

O `PUT` foi validado separadamente com sucesso, mas não foi incluído no fluxo E2E final devido à volatilidade observada no ambiente compartilhado durante uma das execuções.

---

## 8. Observações

### 8.1 Ambiente compartilhado

A API utilizada é pública e compartilhada.

Durante diferentes execuções, alguns identificadores deixaram de estar disponíveis posteriormente ou apresentaram dados diferentes dos inicialmente observados.

Isso pode dificultar a reprodução de cenários que dependem de dados persistentes.

### 8.2 Autenticação

Foram observadas respostas `403 Forbidden` durante algumas operações autenticadas.

Após a renovação do token, algumas operações puderam ser executadas normalmente.

O comportamento não foi automaticamente classificado como defeito.

### 8.3 PATCH

Durante uma tentativa de execução do `PATCH`, foi observada uma resposta `405 Method Not Allowed`.

Após revisão da requisição e nova execução com registro válido, o `PATCH` foi processado com sucesso.

O comportamento não foi classificado como defeito devido à ausência de reprodução consistente.

---

## 9. Defeitos confirmados

Até o momento:

**Nenhum defeito funcional confirmado.**

As ocorrências observadas foram analisadas considerando:

- configuração da requisição;
- autenticação;
- dados utilizados;
- estado do ambiente;
- possibilidade de reprodução.

---

## 10. Limitações

- API pública de demonstração;
- ambiente compartilhado;
- dados sujeitos a alteração;
- identificadores que podem deixar de estar disponíveis;
- validade limitada dos tokens;
- execução manual pelo Postman;
- cobertura ainda parcial dos casos planejados.

---

## 11. Conclusão

Os testes realizados permitiram validar os principais recursos da Restful Booker API relacionados ao ciclo de vida de uma reserva.

As operações de autenticação, consulta, criação, atualização completa, atualização parcial e exclusão foram executadas individualmente com resultados satisfatórios nos cenários positivos avaliados.

Também foi executado um fluxo integrado envolvendo criação, consulta, atualização parcial e exclusão.

As ocorrências observadas durante a execução foram registradas e analisadas separadamente, evitando classificá-las como defeitos sem evidência suficiente.

A execução dos casos de teste continuará para ampliar a cobertura dos cenários negativos e complementar a avaliação da API.
