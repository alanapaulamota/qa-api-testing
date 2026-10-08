# Test Plan — Restful Booker API

## 1. Objetivo

Este projeto tem como objetivo realizar testes de uma API REST, utilizando o Postman, com foco na validação funcional dos endpoints, autenticação, códigos de resposta HTTP, estrutura dos dados retornados e comportamento da API em cenários positivos e negativos.

O projeto também busca demonstrar a aplicação de práticas de planejamento, execução, documentação e análise de testes de API.

## 2. Escopo

Serão avaliados os seguintes recursos disponibilizados pela API Restful Booker:

- Autenticação;
- Consulta de reservas;
- Criação de reservas;
- Atualização completa de reservas;
- Atualização parcial de reservas;
- Exclusão de reservas.

Os testes considerarão:

- Métodos HTTP;
- Códigos de status;
- Headers;
- Estrutura das respostas JSON;
- Campos obrigatórios;
- Tipos e valores dos dados;
- Autenticação e autorização;
- Cenários positivos e negativos;
- Consistência dos dados durante o ciclo de vida de uma reserva.

## 3. Fora de escopo

Não fazem parte deste projeto:

- Testes de interface gráfica;
- Testes de performance ou carga;
- Testes de segurança aprofundados;
- Testes de infraestrutura;
- Testes de banco de dados;
- Avaliação da implementação interna da API.

## 4. Sistema sob teste

**API:** Restful Booker API

**Tipo:** REST API

**Formato de dados:** JSON

**Ferramenta de testes:** Postman

Os testes serão executados contra a API pública disponibilizada pelo projeto Restful Booker.

## 5. Estratégia de testes

A estratégia será baseada na execução de cenários funcionais e não funcionais relacionados ao comportamento observável da API.

Serão considerados:

### Testes positivos

Verificar o comportamento esperado quando a API recebe requisições válidas.

Exemplos:

- autenticação com credenciais válidas;
- consulta de uma reserva existente;
- criação de uma reserva com dados válidos;
- atualização de uma reserva existente;
- exclusão de uma reserva.

### Testes negativos

Verificar o comportamento da API diante de entradas inválidas ou condições não esperadas.

Exemplos:

- credenciais inválidas;
- identificador inexistente;
- dados obrigatórios ausentes;
- valores inválidos;
- token inválido ou ausente;
- requisições com parâmetros incorretos.

### Validação das respostas

Para cada cenário aplicável serão avaliados:

- código de status HTTP;
- headers;
- `Content-Type`;
- estrutura do corpo da resposta;
- presença de campos esperados;
- tipos dos valores;
- valores retornados;
- consistência entre requisições relacionadas.

## 6. Tipos de teste

Serão utilizados neste projeto:

- Testes funcionais;
- Testes de integração entre requisições relacionadas;
- Testes positivos;
- Testes negativos;
- Testes de validação de contrato/resposta;
- Testes de regressão durante a execução da collection.

## 7. Ambiente

Os testes serão executados utilizando:

| Componente          | Ferramenta     |
| ------------------- | -------------- |
| Sistema operacional | Ubuntu Linux   |
| Cliente de API      | Postman        |
| API                 | Restful Booker |
| Formato             | JSON           |
| Controle de versão  | Git            |
| Repositório         | GitHub         |

## 8. Critérios de entrada

A execução dos testes será iniciada quando:

- a API estiver acessível;
- os endpoints necessários estiverem disponíveis;
- o ambiente do Postman estiver configurado;
- os dados necessários para execução estiverem definidos;
- os cenários de teste estiverem documentados.

## 9. Critérios de saída

A execução será considerada concluída quando:

- todos os casos de teste planejados tiverem sido executados;
- os resultados tiverem sido registrados;
- falhas identificadas tiverem sido analisadas;
- defeitos relevantes tiverem sido documentados;
- os resultados gerais dos testes tiverem sido consolidados no relatório final.

## 10. Riscos e limitações

Por se tratar de uma API pública, o ambiente de testes está sujeito a alterações, indisponibilidade temporária e limitações que não estão sob controle do projeto.

Além disso, o comportamento observado durante os testes representa a versão e as condições da API no momento da execução.

Eventuais diferenças entre o comportamento observado e o comportamento esperado serão analisadas antes de serem classificadas como defeitos.
