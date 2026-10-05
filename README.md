# API REST de Gerenciamento de Professores

API REST desenvolvida em **Java com Spring Boot, Spring Data JPA e PostgreSQL** para cadastro e gerenciamento de professores.

O projeto tem como objetivo praticar a criação de uma API REST, persistência de dados com JPA, operações de CRUD e consultas utilizando métodos derivados do Spring Data JPA.

---

## 👨‍🎓 Identificação

**Aluno:** Nicolas Alexandrino da Silva Amorim

**Projeto:** API REST de Gerenciamento de Professores

---

## 📋 Descrição

A API permite realizar o gerenciamento de professores, possibilitando:

* Listar professores;
* Buscar professores pelo nome;
* Buscar professores pela área de atuação;
* Cadastrar professores;
* Editar professores;
* Excluir professores.

A aplicação utiliza arquitetura em camadas, separando Controller, Service, Repository e Model.

---

## 🛠️ Tecnologias utilizadas

* Java
* Spring Boot
* Spring Web
* Spring Data JPA
* Hibernate/JPA
* PostgreSQL
* Maven
* Insomnia
* Git e GitHub

---

## 📁 Estrutura do projeto

```text
src/
└── main/
    ├── java/
    │   └── com/API/APItop/
    │       ├── Controller/
    │       │   └── ProfessorController.java
    │       ├── Model/
    │       │   └── Professor.java
    │       ├── Repository/
    │       │   └── ProfessorRepository.java
    │       ├── Service/
    │       │   └── ProfessorService.java
    │       └── ApItopApplication.java
    │
    └── resources/
        ├── application.properties
        ├── schema.sql
        └── data.sql
```

---

## 🗄️ Banco de dados

O projeto utiliza **PostgreSQL**.

Banco de dados:

```text
APItop
```

Tabela:

```text
professor
```

Estrutura:

| Campo    | Tipo         | Descrição                  |
| -------- | ------------ | -------------------------- |
| id       | BIGSERIAL    | Identificador do professor |
| nome     | VARCHAR(100) | Nome completo              |
| email    | VARCHAR(150) | E-mail                     |
| area     | VARCHAR(100) | Área de atuação            |
| telefone | VARCHAR(20)  | Telefone                   |

A tabela possui `id` como chave primária.

O projeto também possui os arquivos `schema.sql` e `data.sql` para criação da tabela e inserção de dados iniciais.

---

## ⚙️ Configuração do banco

A aplicação utiliza PostgreSQL local.

Configuração padrão:

```text
Banco: APItop
Host: localhost
Porta: 5432
Usuário: postgres
```

As configurações podem ser alteradas no arquivo:

```text
src/main/resources/application.properties
```

> Não publique senhas reais do PostgreSQL no GitHub.

---

# ▶️ Como executar

### 1. Pré-requisitos

Instale:

* Java
* PostgreSQL
* Git
* Maven ou utilize o Maven Wrapper incluído no projeto.

### 2. Criar o banco

No PostgreSQL/pgAdmin, crie um banco chamado:

```text
APItop
```

### 3. Executar a aplicação

No terminal, dentro da pasta do projeto:

```bash
./mvnw spring-boot:run
```

No Windows:

```powershell
.\mvnw.cmd spring-boot:run
```

A API estará disponível em:

```text
http://localhost:8080
```

---

# 🔌 Endpoints

## 1. Listar todos os professores

**GET**

```text
/professores
```

Exemplo:

```text
GET http://localhost:8080/professores
```

Retorna todos os professores cadastrados.

---

## 2. Filtrar professores por nome

**GET**

```text
/professores/nome/{nome}
```

Exemplo:

```text
GET http://localhost:8080/professores/nome/joao
```

A busca é parcial e não diferencia letras maiúsculas de minúsculas.

Exemplo:

```text
João da Silva
João Pedro
Maria Joana
```

Podem ser encontrados através de uma busca por:

```text
joao
```

O Repository utiliza:

```java
findByNomeContainingIgnoreCase(...)
```

---

## 3. Filtrar professores por área

**GET**

```text
/professores/area/{area}
```

Exemplo:

```text
GET http://localhost:8080/professores/area/desenvolvimento
```

A busca não diferencia letras maiúsculas de minúsculas.

O Repository utiliza:

```java
findByAreaIgnoreCase(...)
```

---

## 4. Cadastrar professor

**POST**

```text
/professores
```

Exemplo:

```text
POST http://localhost:8080/professores
```

### JSON

```json
{
    "nome": "Maria Silva",
    "email": "maria@email.com",
    "area": "Desenvolvimento",
    "telefone": "86999999999"
}
```

O professor será salvo no banco de dados e receberá um `id` automaticamente.

---

## 5. Editar professor

**PUT**

```text
/professores/{id}
```

Exemplo:

```text
PUT http://localhost:8080/professores/1
```

### JSON

```json
{
    "nome": "Maria Silva Santos",
    "email": "mariasantos@email.com",
    "area": "Engenharia de Software",
    "telefone": "86988888888"
}
```

Atualiza os dados do professor correspondente ao `id` informado.

---

## 6. Excluir professor

**DELETE**

```text
/professores/{id}
```

Exemplo:

```text
DELETE http://localhost:8080/professores/1
```

Remove o professor correspondente ao `id` informado.

---

# 📊 Resumo dos endpoints

| Método | Endpoint                   | Descrição                   |
| ------ | -------------------------- | --------------------------- |
| GET    | `/professores`             | Lista todos os professores  |
| GET    | `/professores/nome/{nome}` | Filtra professores por nome |
| GET    | `/professores/area/{area}` | Filtra professores por área |
| POST   | `/professores`             | Cadastra um professor       |
| PUT    | `/professores/{id}`        | Edita um professor          |
| DELETE | `/professores/{id}`        | Exclui um professor         |

---

# 🔎 Consultas com Spring Data JPA

As consultas de nome e área foram implementadas utilizando **métodos derivados do Spring Data JPA**, sem utilização de `@Query`.

### Busca por nome

```java
findByNomeContainingIgnoreCase(String nome)
```

O método utiliza:

* `Containing` → permite busca parcial;
* `IgnoreCase` → ignora diferenças entre maiúsculas e minúsculas.

### Busca por área

```java
findByAreaIgnoreCase(String area)
```

O método utiliza:

* `IgnoreCase` → ignora diferenças entre maiúsculas e minúsculas.

---

# 🏗️ Arquitetura

A aplicação está organizada em camadas:

```text
Cliente
   ↓
Controller
   ↓
Service
   ↓
Repository
   ↓
PostgreSQL
```

### Model

Responsável pela entidade `Professor` e seu mapeamento para a tabela `professor`.

### Repository

Responsável pelo acesso aos dados utilizando Spring Data JPA.

### Service

Responsável pela lógica e operações da aplicação.

### Controller

Responsável pelos endpoints REST e comunicação com o cliente.

---

# 🧪 Testes

Os endpoints foram testados utilizando o **Insomnia**.

Foram realizados testes para:

* Listagem de professores;
* Filtro por nome;
* Filtro por área;
* Cadastro de professor;
* Edição de professor;
* Exclusão de professor.

## Evidências

### 1. Listar professores

```text
GET /professores
```
<img width="1909" height="986" alt="Captura de tela 2026-10-04 224435" src="https://github.com/user-attachments/assets/9f0f2550-299b-45f4-8eab-ec7156c38433" />


### 2. Filtrar por nome

```text
GET /professores/nome/{nome}
```
<img width="949" height="930" alt="Captura de tela 2026-10-04 224612" src="https://github.com/user-attachments/assets/75605e1d-441c-4cb6-a5ee-4ed1da88e08a" />

### 3. Filtrar por área

```text
GET /professores/area/{area}
```
<img width="951" height="964" alt="Captura de tela 2026-10-04 224711" src="https://github.com/user-attachments/assets/f6130560-62c8-48a9-ad1a-63fb72659378" />


### 4. Cadastrar professor

```text
POST /professores
```
<img width="942" height="941" alt="Captura de tela 2026-10-04 224803" src="https://github.com/user-attachments/assets/16ef084d-1b6d-4b1a-b61f-1a51618a93b6" />


### 5. Editar professor

```text
PUT /professores/{id}
```
<img width="950" height="963" alt="Captura de tela 2026-10-04 224842" src="https://github.com/user-attachments/assets/b9953349-4358-4200-9362-416a227f5ba8" />

### 6. Excluir professor

```text
DELETE /professores/{id}
```
<img width="947" height="956" alt="Captura de tela 2026-10-04 225007" src="https://github.com/user-attachments/assets/af066df0-be3e-4677-a69f-6a98c0e6db9d" />


---

# 📦 Publicação no GitHub

O projeto deve ser disponibilizado em um repositório público no GitHub.

Exemplo de comandos:

```bash
git init
git add .
git commit -m "Implementa API REST de professores"
git branch -M main
git remote add origin URL_DO_REPOSITORIO
git push -u origin main
```

Arquivos gerados automaticamente não devem ser enviados ao GitHub, como:

```text
target/
.idea/
```

---




## 🎯 Resultado

API REST completa para gerenciamento de professores, permitindo **cadastrar, consultar, filtrar, editar e excluir professores** utilizando **Spring Boot, Spring Data JPA e PostgreSQL**.
