# Test Cases — Restful Booker API

## 1. Identificação

**Projeto:** QA API Testing  
**Sistema sob teste:** Restful Booker API  
**Ferramenta:** Postman  
**Versão:** 1.0  
**Status:** Em execução

---

## 2. Convenção de identificação

Os casos de teste utilizam identificadores compostos pela funcionalidade e pelo número sequencial:

- `TC-AUTH-XXX` — Autenticação
- `TC-BOOK-LIST-XXX` — Listagem de reservas
- `TC-BOOK-GET-XXX` — Consulta de reserva
- `TC-BOOK-CREATE-XXX` — Criação de reserva
- `TC-BOOK-PUT-XXX` — Atualização completa
- `TC-BOOK-PATCH-XXX` — Atualização parcial
- `TC-BOOK-DELETE-XXX` — Exclusão
- `TC-E2E-XXX` — Fluxos End-to-End

---

# 3. Authentication

## TC-AUTH-001 — Gerar token com credenciais válidas

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Pré-condições

Nenhuma.

### Requisição

```http
POST /auth
Content-Type: application/json
```

### Dados de entrada

```json
{
  "username": "admin",
  "password": "{{password}}"
}
```

### Resultado esperado

- A requisição deve ser processada com sucesso.
- A resposta deve conter um token de autenticação.
- O token deve poder ser utilizado nas operações que exigem autenticação.

### Resultado da execução

- Resposta recebida com status `200 OK`.
- Token retornado pela API.
- Token armazenado automaticamente na variável de ambiente `token`.

---

## TC-AUTH-002 — Gerar token com usuário inválido

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

```json
{
  "username": "invalid_user",
  "password": "{{password}}"
}
```

### Resultado esperado

- A autenticação não deve gerar um token válido.
- A resposta deve indicar que as credenciais não são válidas.

---

## TC-AUTH-003 — Gerar token com senha inválida

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

```json
{
  "username": "admin",
  "password": "invalid_password"
}
```

### Resultado esperado

- A autenticação não deve gerar um token válido.
- A resposta deve indicar falha na autenticação.

---

## TC-AUTH-004 — Solicitar autenticação sem credenciais

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

```json
{}
```

### Resultado esperado

- A API deve rejeitar ou indicar a ausência das credenciais necessárias.
- Nenhum token válido deve ser disponibilizado.

---

# 4. Booking — Listagem

## TC-BOOK-LIST-001 — Listar reservas

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Requisição

```http
GET /booking
```

### Resultado esperado

- A requisição deve retornar a lista de reservas.
- A resposta deve possuir formato JSON.
- Cada item retornado deve possuir um identificador de reserva.

### Resultado da execução

- Resposta recebida com status `200 OK`.
- Resposta em formato JSON.
- A resposta foi validada como array.
- Os itens retornados foram validados quanto à presença de `bookingid`.

---

## TC-BOOK-LIST-002 — Filtrar reservas por nome

**Prioridade:** Média  
**Tipo:** Positivo  
**Status:** Not Run

### Requisição

```http
GET /booking?firstname={firstname}&lastname={lastname}
```

### Resultado esperado

- A API deve retornar as reservas correspondentes aos parâmetros informados.
- Os resultados retornados devem possuir identificadores válidos.

---

# 5. Booking — Consulta por ID

## TC-BOOK-GET-001 — Consultar reserva existente

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Pré-condições

Possuir um `bookingId` válido.

### Requisição

```http
GET /booking/{bookingId}
```

### Resultado esperado

- A reserva deve ser localizada.
- A resposta deve retornar os dados da reserva.
- Os campos retornados devem possuir a estrutura esperada.

### Resultado da execução

- Reserva criada durante a execução foi consultada com sucesso.
- Resposta recebida com status `200 OK`.
- Estrutura e tipos dos principais campos foram validados por testes automatizados no Postman.

---

## TC-BOOK-GET-002 — Consultar reserva inexistente

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Pass

### Requisição

```http
GET /booking/{bookingId}
```

### Dados de entrada

Utilizar um identificador que não corresponda a uma reserva existente.

### Resultado esperado

- A API deve indicar que o recurso solicitado não foi encontrado.
- A resposta não deve retornar uma reserva inexistente.

### Resultado da execução

- Identificador `2833`, após exclusão da reserva, foi consultado novamente.
- A API retornou `404 Not Found`.
- O comportamento observado correspondeu ao esperado.

---

## TC-BOOK-GET-003 — Consultar reserva com identificador inválido

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

Utilizar um identificador em formato incompatível com o endpoint.

Exemplo:

```text
/booking/abc
```

### Resultado esperado

- A API deve tratar o identificador inválido adequadamente.
- A resposta não deve retornar uma reserva válida associada ao identificador informado.

---

# 6. Booking — Criação

## TC-BOOK-CREATE-001 — Criar reserva com dados válidos

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Requisição

```http
POST /booking
Content-Type: application/json
```

### Dados de entrada

```json
{
  "firstname": "Alana",
  "lastname": "Test",
  "totalprice": 150,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-11-01",
    "checkout": "2026-11-05"
  },
  "additionalneeds": "Breakfast"
}
```

### Resultado esperado

- A reserva deve ser criada.
- A resposta deve fornecer um identificador para a nova reserva.
- Os dados retornados devem corresponder aos dados enviados.

### Resultado da execução

- Reserva criada com sucesso.
- Identificador retornado pela API.
- Dados da reserva foram validados na resposta.
- O `bookingId` foi armazenado automaticamente em variável de ambiente pelo script de Postman.

---

## TC-BOOK-CREATE-002 — Criar reserva sem nome

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

Remover o campo `firstname` do payload.

### Resultado esperado

- A API deve rejeitar ou tratar adequadamente a ausência do campo.
- A criação de uma reserva incompleta não deve ser considerada um sucesso funcional.

---

## TC-BOOK-CREATE-003 — Criar reserva sem sobrenome

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

Remover o campo `lastname`.

### Resultado esperado

- A API deve rejeitar ou tratar adequadamente a ausência do campo.
- A resposta deve ser analisada quanto ao comportamento da validação.

---

## TC-BOOK-CREATE-004 — Criar reserva com tipo de dado inválido

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

Enviar um tipo incompatível para um campo que espera outro tipo.

Exemplo:

```json
{
  "firstname": 12345
}
```

### Resultado esperado

- A API deve validar o tipo do dado recebido.
- O comportamento observado será comparado com o comportamento esperado e documentado.

---

## TC-BOOK-CREATE-005 — Criar reserva com datas inválidas

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Dados de entrada

Enviar datas em formato inválido.

Exemplo:

```json
{
  "bookingdates": {
    "checkin": "invalid-date",
    "checkout": "invalid-date"
  }
}
```

### Resultado esperado

- A API deve validar os valores das datas.
- Uma reserva com datas inválidas não deve ser considerada válida.

---

# 7. Booking — Atualização completa

## TC-BOOK-PUT-001 — Atualizar reserva existente

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Pré-condições

- Possuir `bookingId` válido.
- Possuir token de autenticação válido.

### Requisição

```http
PUT /booking/{bookingId}
Content-Type: application/json
Accept: application/json
Cookie: token={token}
```

### Resultado esperado

- A reserva deve ser atualizada.
- A resposta deve representar os novos dados da reserva.
- Uma consulta posterior deve confirmar a alteração.

### Resultado da execução

- Atualização realizada com sucesso.
- Resposta recebida com status `200 OK`.
- Os campos atualizados foram validados por testes automatizados no Postman.
- Durante a execução, foi necessário renovar o token após uma resposta `403 Forbidden`.

### Observação

O ambiente de demonstração apresentou comportamento volátil em relação aos identificadores de reserva durante diferentes execuções. Por esse motivo, a confirmação posterior da alteração deve ser interpretada considerando o estado compartilhado do ambiente.

---

## TC-BOOK-PUT-002 — Atualizar reserva sem autenticação

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Pré-condições

Possuir `bookingId` válido.

### Resultado esperado

- A API deve impedir a operação sem autenticação válida.
- A reserva não deve ser alterada.

---

## TC-BOOK-PUT-003 — Atualizar reserva com token inválido

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Resultado esperado

- A API deve impedir a operação.
- A reserva não deve ser alterada.

---

# 8. Booking — Atualização parcial

## TC-BOOK-PATCH-001 — Atualizar parcialmente uma reserva

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Pré-condições

- Possuir `bookingId` válido.
- Possuir token válido.

### Dados de entrada

Enviar somente os campos que devem ser alterados.

Exemplo:

```json
{
  "firstname": "UpdatedName"
}
```

### Resultado esperado

- O campo informado deve ser atualizado.
- Os demais dados da reserva devem permanecer inalterados.

### Resultado da execução

- O campo `firstname` foi atualizado com sucesso.
- A resposta foi recebida com status `200 OK`.
- Os demais campos foram validados e permaneceram com os valores esperados.
- Testes automatizados foram executados no Postman.

---

## TC-BOOK-PATCH-002 — Atualizar parcialmente sem autenticação

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Resultado esperado

- A operação deve ser impedida sem autenticação válida.
- Nenhum dado deve ser alterado.

---

## TC-BOOK-PATCH-003 — Atualizar parcialmente com token inválido

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Resultado esperado

- A API deve impedir a alteração.
- Os dados da reserva devem permanecer inalterados.

---

# 9. Booking — Exclusão

## TC-BOOK-DELETE-001 — Excluir reserva existente

**Prioridade:** Alta  
**Tipo:** Positivo  
**Status:** Pass

### Pré-condições

- Possuir `bookingId` válido.
- Possuir token válido.

### Requisição

```http
DELETE /booking/{bookingId}
Cookie: token={token}
```

### Resultado esperado

- A reserva deve ser excluída.
- Uma consulta posterior pelo mesmo ID deve indicar que a reserva não está mais disponível.

### Resultado da execução

- Reserva excluída com sucesso.
- API retornou `201 Created`.
- Consulta posterior pelo mesmo identificador retornou `404 Not Found`.
- O comportamento observado correspondeu ao esperado.

---

## TC-BOOK-DELETE-002 — Excluir reserva sem autenticação

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Resultado esperado

- A operação deve ser impedida.
- A reserva deve permanecer disponível.

---

## TC-BOOK-DELETE-003 — Excluir reserva com token inválido

**Prioridade:** Alta  
**Tipo:** Negativo  
**Status:** Not Run

### Resultado esperado

- A operação deve ser impedida.
- A reserva deve permanecer disponível.

---

## TC-BOOK-DELETE-004 — Excluir reserva inexistente

**Prioridade:** Média  
**Tipo:** Negativo  
**Status:** Not Run

### Resultado esperado

- A API deve tratar adequadamente a tentativa de exclusão de um recurso inexistente.
- Nenhum recurso existente deve ser afetado.

---

# 10. End-to-End — Ciclo de vida da reserva

## TC-E2E-001 — Executar ciclo de vida de uma reserva

**Prioridade:** Crítica  
**Tipo:** Integração / End-to-End  
**Status:** Pass — fluxo parcial executado

### Fluxo executado

```text
1. Criar reserva
2. Armazenar bookingId
3. Consultar reserva
4. Atualizar reserva parcialmente com PATCH
5. Excluir reserva
6. Consultar reserva após exclusão
```

### Resultado esperado

Cada etapa deve utilizar o resultado da etapa anterior quando aplicável.

O fluxo deve demonstrar que:

- a reserva foi criada;
- o identificador foi obtido;
- os dados podem ser consultados;
- a atualização parcial foi aplicada;
- a reserva foi excluída;
- a reserva não está mais disponível após a exclusão.

### Resultado da execução

O fluxo foi executado com uma reserva criada durante a própria execução:

```text
Create → Get → PATCH → Delete → Get
```

O identificador utilizado foi `2937`.

Resultados observados:

- `Create`: sucesso;
- `Get`: sucesso;
- `PATCH`: sucesso;
- `Delete`: sucesso, com resposta `201 Created`;
- `Get após Delete`: `404 Not Found`.

### Observação

O fluxo originalmente planejado também previa uma atualização completa com `PUT`. O `PUT` foi validado separadamente com sucesso, porém não foi mantido no fluxo E2E final devido à volatilidade observada no ambiente compartilhado durante uma das execuções. Por esse motivo, o resultado do E2E é considerado **fluxo parcial executado**, e não como evidência de execução de todas as etapas originalmente planejadas.

---

# 11. Observações de execução

Os resultados esperados acima representam o comportamento funcional esperado para os cenários.

Durante a execução, o **resultado real** foi registrado para os casos executados. Quando a API apresentou comportamento diferente do esperado, o resultado não foi automaticamente classificado como defeito.

Durante os testes, foram observados:

- respostas `403 Forbidden` relacionadas à autenticação em operações que exigiam token;
- necessidade de renovação do token durante algumas execuções;
- uma ocorrência de `405 Method Not Allowed` durante uma tentativa de PATCH;
- comportamento volátil dos dados e identificadores no ambiente compartilhado;
- identificadores de reservas que deixaram de estar disponíveis entre diferentes requisições ou execuções.

Essas ocorrências foram tratadas como **observações de execução** e não foram automaticamente classificadas como defeitos da API.

A análise considera:

- documentação da API;
- comportamento observado;
- consistência da resposta;
- contexto da execução;
- impacto funcional;
- possibilidade de comportamento conhecido do ambiente de demonstração.

Somente após essa análise o comportamento poderá ser registrado como defeito ou observação.

---

# 12. Status dos casos

| Status         | Significado                                     |
| -------------- | ----------------------------------------------- |
| Not Run        | Caso ainda não executado                        |
| Pass           | Resultado observado corresponde ao esperado     |
| Fail           | Resultado observado não corresponde ao esperado |
| Blocked        | Não foi possível executar o caso                |
| Not Applicable | Caso não aplicável ao ambiente ou cenário       |

---

# 13. Resumo da execução

Até o momento, foram executados com sucesso os seguintes cenários:

| Caso                 | Resultado            |
| -------------------- | -------------------- |
| `TC-AUTH-001`        | Pass                 |
| `TC-BOOK-LIST-001`   | Pass                 |
| `TC-BOOK-GET-001`    | Pass                 |
| `TC-BOOK-GET-002`    | Pass                 |
| `TC-BOOK-CREATE-001` | Pass                 |
| `TC-BOOK-PUT-001`    | Pass                 |
| `TC-BOOK-PATCH-001`  | Pass                 |
| `TC-BOOK-DELETE-001` | Pass                 |
| `TC-E2E-001`         | Pass — fluxo parcial |

Os demais casos permanecem como **Not Run** e poderão ser executados em uma etapa posterior.
