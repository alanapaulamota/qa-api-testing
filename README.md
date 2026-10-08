# QA API Testing - Restful Booker

Projeto de testes de API REST desenvolvido com **Postman**, utilizando a API pública [Restful Booker](https://restful-booker.herokuapp.com/).

O objetivo do projeto é demonstrar, de forma prática, conhecimentos em **testes funcionais de APIs, validação de respostas, autenticação, operações CRUD, testes de atualização, testes end-to-end e documentação de qualidade**.

---

## 🎯 Objetivos

- Validar endpoints de uma API REST.
- Verificar códigos de status HTTP e estrutura das respostas.
- Validar tipos e valores dos dados retornados.
- Testar autenticação baseada em token.
- Validar operações de criação, consulta, atualização e exclusão.
- Realizar testes de atualização parcial com `PATCH`.
- Executar um fluxo end-to-end utilizando dados criados durante o teste.
- Registrar casos de teste, evidências, observações e resultados.

---

## 🧪 Tecnologias e ferramentas

- **Postman**
- **JavaScript** — scripts de teste do Postman
- **REST API**
- **JSON**
- **HTTP**
- **Git / GitHub**
- **Markdown**

---

## 📌 API testada

**Restful Booker API**

Documentação da API:

https://restful-booker.herokuapp.com/apidoc/index.html

Principais endpoints utilizados:

| Método | Endpoint | Objetivo |
|---|---|---|
| `POST` | `/auth` | Gerar token de autenticação |
| `GET` | `/booking` | Listar reservas |
| `GET` | `/booking/{id}` | Consultar reserva |
| `POST` | `/booking` | Criar reserva |
| `PUT` | `/booking/{id}` | Atualizar reserva |
| `PATCH` | `/booking/{id}` | Atualizar parcialmente |
| `DELETE` | `/booking/{id}` | Excluir reserva |

---

## 📂 Estrutura do projeto

```text
qa-api-testing/
├── docs/
│   ├── bug-reports.md
│   ├── test-cases.md
│   ├── test-plan.md
│   └── test-report.md
├── postman/
│   ├── collections/
│   │   └── QA API Testing - Restful Booker.postman_collection.json
│   └── environments/
│       └── Restful Booker - QA.postman_environment.json
├── reports/
│   └── .gitkeep
├── test-data/
│   └── .gitkeep
├── .gitignore
└── README.md
```

---

## 🔎 Estratégia de testes

A estratégia utilizada contempla diferentes operações e cenários da API:

### Autenticação

- Geração de token.
- Validação do código de resposta.
- Validação do formato JSON.
- Verificação da presença do token.

### Consulta

- Listagem de reservas.
- Consulta de reserva por ID.
- Validação de campos obrigatórios.
- Validação de tipos de dados.

### Criação

- Criação de uma nova reserva.
- Validação do ID retornado.
- Validação dos dados enviados e retornados.

### Atualização

- Atualização completa utilizando `PUT`.
- Atualização parcial utilizando `PATCH`.
- Validação dos dados alterados.
- Verificação da preservação dos campos não alterados.

### Exclusão

- Exclusão de reserva.
- Validação da resposta da API.
- Nova consulta após exclusão para verificar o comportamento do recurso removido.

### End-to-End

Foi executado um fluxo utilizando uma reserva criada durante o teste:

```text
Create
   ↓
Get
   ↓
PATCH
   ↓
Delete
   ↓
Get → 404
```

O fluxo foi validado utilizando dados gerados durante a execução, evitando depender de um ID fixo previamente existente na API.

---

## ✅ Validações automatizadas

Os requests possuem scripts JavaScript no Postman para validar, entre outros:

- Código de status HTTP.
- Resposta em JSON.
- Existência de propriedades.
- Tipos de dados.
- Valores esperados.
- Estrutura de objetos.
- Persistência das alterações.
- Preservação de dados durante atualização parcial.

Exemplo:

```javascript
pm.test("Response status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});
```

Em operações de criação, o ID retornado pela API também é armazenado automaticamente no ambiente:

```javascript
const response = pm.response.json();

if (response.bookingid) {
    pm.environment.set("bookingId", response.bookingid);
}
```

Isso permite reutilizar o identificador em requisições posteriores.

---

## 📊 Resultados

Durante a execução foram validados cenários de:

- Autenticação
- Listagem
- Consulta
- Criação
- Atualização completa
- Atualização parcial
- Exclusão
- Fluxo end-to-end

Os casos executados apresentaram respostas compatíveis com os critérios definidos para os cenários testados.

Os resultados detalhados estão disponíveis em:

- [`docs/test-plan.md`](docs/test-plan.md)
- [`docs/test-cases.md`](docs/test-cases.md)
- [`docs/bug-reports.md`](docs/bug-reports.md)
- [`docs/test-report.md`](docs/test-report.md)

---

## ⚠️ Observações sobre o ambiente

A Restful Booker é uma API pública de demonstração e possui comportamento compartilhado e potencialmente volátil.

Durante os testes foram observadas situações como:

- Registros criados que posteriormente deixaram de estar disponíveis.
- Respostas `403` durante operações autenticadas, solucionadas após renovação do token em determinados cenários.
- Uma ocorrência de `405` durante uma tentativa de `PATCH`, posteriormente resolvida após revisão/reabertura da requisição no Postman.

Essas ocorrências foram tratadas como **observações do ambiente**, e não como defeitos funcionais confirmados, quando não houve evidência suficiente de comportamento reproduzível da aplicação.

---

## 📋 Documentação de QA

### Plano de testes

Define objetivo, escopo, estratégia, ambiente, dados, critérios e riscos.

➡️ [`test-plan.md`](docs/test-plan.md)

### Casos de teste

Documenta os cenários planejados, critérios de aceite e resultados das execuções.

➡️ [`test-cases.md`](docs/test-cases.md)

### Registro de defeitos e observações

Documenta defeitos confirmados e comportamentos observados durante os testes.

➡️ [`bug-reports.md`](docs/bug-reports.md)

### Relatório de testes

Apresenta o resumo da execução, resultados e limitações encontradas.

➡️ [`test-report.md`](docs/test-report.md)

---

## 🚀 Como executar

### 1. Clone o projeto

```bash
git clone <URL_DO_REPOSITORIO>
cd qa-api-testing
```

### 2. Abra o Postman

Importe a collection localizada em:

```text
postman/collections/
```

Importe também o environment:

```text
postman/environments/
```

### 3. Configure o ambiente

Selecione:

```text
Restful Booker - QA
```

Preencha localmente os valores necessários:

```text
baseUrl
username
password
```

O `token` pode ser gerado pelo request de autenticação.

O `bookingId` pode ser atualizado automaticamente pelos scripts de criação.

> O arquivo versionado no repositório não contém os valores reais das credenciais.

---

## 📈 Próximos passos

Possíveis evoluções deste projeto:

- Execução da collection via Newman.
- Integração com pipeline CI/CD.
- Geração de relatórios automatizados.
- Ampliação da cobertura de cenários negativos.
- Testes de contrato.
- Automação complementar utilizando Java + Rest Assured.

---

## 👩‍💻 Sobre o projeto

Projeto desenvolvido como parte do portfólio profissional de **QA e Testes de Software**, com foco em testes de APIs REST, documentação e validação automatizada de respostas.

**Alana Paula Barbosa Mota**  
Mestre em Ciência da Computação — UnB
