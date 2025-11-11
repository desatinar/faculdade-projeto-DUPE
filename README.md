# Projeto: API de Duplicata Escritural com Flask

## Objetivo

Desenvolver uma API RESTful em Flask para cadastrar, buscar e alterar o status de duplicatas escriturais (emissão, aceite, liquidação), simulando um microserviço básico de registro e consulta desses títulos.

## 1. Fluxograma (Diagrama de Classes UML)

Abaixo está o diagrama de classes UML que define a estrutura dos dados do banco de dados e a relação entre as entidades `Empresa` e `Duplicata`.
*   **Empresa** atua como a tabela para Sacadores e Sacados.
*   **Duplicata** se relaciona duas vezes com a `Empresa` através das chaves estrangeiras `sacador_id` e `sacado_id`.
*   As relações **emite** e **recebe** são ambas de 1 (`Empresa`) para 0..n (Muitas `Duplicatas`), indicando que uma empresa pode emitir e receber múltiplas duplicatas.
  <img width="772" height="405" alt="Diagrama sem nome" src="https://github.com/user-attachments/assets/87c55d70-0f00-477e-b6e0-f9fd61f1bc22" />

## 2. Especificação das Funcionalidades

A API será estruturada em dois grupos de endpoints para gerenciar as entidades e o fluxo de negócio principal.

### Gestão de Empresas (Sacadores/Sacados)

| Método | Endpoint | Descrição | Corpo (JSON) | Retorno |
| :--- | :--- | :--- | :--- | :--- |
| `POST` | `/empresas` | Cadastra uma nova empresa (potencial sacador ou sacado). | `{ "nome_razao_social": "...", "cnpj_cpf": "..." }` | `201` (Objeto da empresa criada) |
| `GET` | `/empresas` | Retorna uma lista de todas as empresas cadastradas. | N/A | `200` (Lista de objetos de empresas) |
| `GET` | `/empresas/<id>` | Retorna os detalhes de uma empresa específica pelo seu ID. | N/A | `200` (Objeto da empresa) |

### Gestão de Duplicatas (Fluxo Principal)


| Método | Endpoint | Descrição | Corpo (JSON) | Retorno |
| :--- | :--- | :--- | :--- | :--- |
| `POST` | `/duplicatas` | Cria (emite) uma nova duplicata. O status inicial será **EMISSAO**. | `{ "numero_nota": "...", "valor": 1500.50, "data_vencimento": "...", "sacador_id": "...", "sacado_id": "..." }` | `201` (Objeto da duplicata criada) |
| `GET` | `/duplicatas/<id>` | Busca e retorna os detalhes de uma duplicata específica pelo ID. | N/A | `200` (Objeto da duplicata) |
| `PUT` | `/duplicatas/<id>/status` | Altera o status de uma duplicata. Este é o endpoint principal para o fluxo de negócio, permitindo a transição (ex: EMISSAO -> ACEITE ou ACEITE -> LIQUIDACAO). | `{ "status": "ACEITE" }` | `200` (Objeto da duplicata com o status atualizado) |

## 3. Requisitos Técnicos

Para a construção deste microserviço, serão utilizadas as seguintes tecnologias e ferramentas:

*   **Linguagem:** Python 3.10+
*   **Framework:** Flask
*   **Banco de Dados:** SQLite 3 (para simplicidade e portabilidade, ideal para um microserviço básico)
*   **ORM (Object-Relational Mapper):** Flask-SQLAlchemy (para mapeamento das classes Python para o banco de dados)
*   **Ambiente:** venv (ambiente virtual do Python para gerenciamento de dependências)
*   **Ferramentas de Teste:** Postman (para testar, validar e apresentar os endpoints da API)
