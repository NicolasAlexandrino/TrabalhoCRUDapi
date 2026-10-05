# API de Gerenciamento de Professores

## Identificação

- **Aluno:** preencher com o nome do aluno
- **Disciplina:** Desenvolvimento Back-Ende com Java
- **Descrição:** API REST para cadastro, consulta, filtragem, atualização e exclusão de professores.

## Tecnologias

- Java 25
- Spring Boot 4
- Spring Web MVC
- Spring Data JPA
- PostgreSQL
- Maven

## Banco de dados

Crie um banco PostgreSQL chamado `APItop`. Na inicialização, a aplicação cria a tabela `professor` e insere dois registros de exemplo, caso ainda não existam. A tabela possui `id BIGSERIAL` como chave primária e os campos `nome`, `email`, `area` e `telefone` com os tamanhos definidos na atividade.

Por padrão, a aplicação usa `localhost:5432`, usuário `postgres` e senha `123`. Configure outros valores pelas variáveis de ambiente `DB_URL`, `DB_USERNAME` e `DB_PASSWORD`. Exemplo no PowerShell:

```powershell
$env:DB_URL = "jdbc:postgresql://localhost:5432/APItop"
$env:DB_USERNAME = "postgres"
$env:DB_PASSWORD = "123"
```

## Como executar

É necessário ter Java 25 e PostgreSQL instalados, com o banco `APItop` criado e acessível.

```powershell
.\mvnw.cmd test
.\mvnw.cmd spring-boot:run
```

A API ficará disponível em `http://localhost:8080`.

## Endpoints

Todas as respostas e requisições usam JSON. Para espaços e caracteres especiais em parâmetros de caminho, use URL encoding.

| Método | Endpoint | Descrição | Sucesso |
| --- | --- | --- | --- |
| GET | `/professores` | Lista todos os professores | 200 |
| GET | `/professores/nome/{nome}` | Busca por trecho do nome, ignorando maiúsculas/minúsculas | 200 |
| GET | `/professores/area/{area}` | Busca por área, ignorando maiúsculas/minúsculas | 200 |
| POST | `/professores` | Cadastra professor | 201 |
| PUT | `/professores/{id}` | Atualiza professor | 200 |
| DELETE | `/professores/{id}` | Exclui professor | 204 |

Exemplo de cadastro:

```json
{
  "nome": "Maria Silva",
  "email": "maria@email.com",
  "area": "Desenvolvimento",
  "telefone": "86999999999"
}
```

Um ID inexistente em `PUT` ou `DELETE` retorna 404.

## Testes

Os testes MockMvc verificam as seis rotas HTTP. Eles isolam a camada web e não substituem uma validação ponta a ponta com PostgreSQL. Execute-os com `.\mvnw.cmd test` no PowerShell.

## Evidências de execução

Os testes automatizados não geram screenshots de uma chamada real ao banco. Para completar a entrega, execute as seis operações no Postman ou Insomnia e salve as capturas em `docs/evidencias/`, mostrando método, URL, status e resposta (e o JSON enviado em POST/PUT). Inclua abaixo as imagens capturadas; nenhuma evidência de execução real foi simulada neste repositório.

- Listar: `docs/evidencias/01-listar-professores.png`
- Filtrar por nome: `docs/evidencias/02-filtrar-nome.png`
- Filtrar por área: `docs/evidencias/03-filtrar-area.png`
- Cadastrar: `docs/evidencias/04-cadastrar-professor.png`
- Editar: `docs/evidencias/05-editar-professor.png`
- Excluir: `docs/evidencias/06-excluir-professor.png`

## Publicação no GitHub

Este diretório precisa ser enviado a um repositório público do GitHub para a entrega. O workspace atual não está inicializado como repositório Git; depois de criar o repositório remoto, publique o projeto com:

```powershell
git init
git add .
git commit -m "Implementa API de gerenciamento de professores"
git branch -M main
git remote add origin URL_DO_REPOSITORIO
git push -u origin main
```