# Loja API

API REST de produtos e categorias, feita com NestJS, Prisma 7 e SQLite.
Projeto da disciplina Backend 2 (Desenvolvimento de APIs com NestJS).

## Requisitos

- Node.js 22 ou superior (confira com `node --version`)
- npm (já vem com o Node)
- Git

## Como rodar

**1. Baixe o projeto**

```bash
git clone https://github.com/Lunnifsp/loja-api.git
cd loja-api
```

**2. Instale as dependências**

```bash
npm install
```

**3. Crie o arquivo de variáveis de ambiente**

```bash
cp .env.example .env
```

**4. Gere o cliente do Prisma e crie o banco**

```bash
npx prisma generate
npx prisma migrate dev
```

Isso cria o arquivo `dev.db` (banco SQLite) com as tabelas `Categoria` e `Produto`.

**5. Inicie o servidor**

```bash
npm run start:dev
```

Quando aparecer `Nest application successfully started`, a API está no ar em
`http://localhost:3000`.

## Rotas

### Categoria

| Método | Rota | O que faz |
|---|---|---|
| POST | `/categoria` | Cria uma categoria |
| GET | `/categoria` | Lista as categorias (com seus produtos) |
| GET | `/categoria/:id` | Busca uma categoria |
| PATCH | `/categoria/:id` | Atualiza uma categoria |
| DELETE | `/categoria/:id` | Remove uma categoria |

### Produto

| Método | Rota | O que faz |
|---|---|---|
| POST | `/produto` | Cria um produto |
| GET | `/produto` | Lista os produtos (com a categoria) |
| GET | `/produto/disponiveis` | Lista só os produtos com estoque maior que 0 |
| GET | `/produto/:id` | Busca um produto |
| PATCH | `/produto/:id` | Atualiza um produto |
| DELETE | `/produto/:id` | Remove um produto |

## Exemplos de corpo (JSON)

Crie a categoria **antes** do produto, porque todo produto precisa de uma `categoriaId`.

Categoria:

```json
{ "nome": "Informática" }
```

Produto:

```json
{
  "nome": "Notebook",
  "descricao": "Notebook para desenvolvimento",
  "preco": 3500,
  "estoque": 10,
  "categoriaId": 1
}
```

Dados inválidos devolvem `400` com a lista de erros, e ids que não existem devolvem `404`.

## Testando com o Bruno

1. Deixe o servidor rodando (`npm run start:dev`).
2. No Bruno (ou na extensão do VS Code), escolha **Open Collection**.
3. Selecione a pasta `bruno/loja-api` deste projeto.
4. Envie as requisições. Comece pelo `POST categoria` e depois o `POST produto`.

No Windows, prefira o Bruno ao `curl`: o `curl` do Git Bash pode quebrar os acentos.

## Ver o banco de dados (Prisma Studio)

No Git Bash do Windows, dentro da pasta do projeto:

```bash
npx prisma studio --url "file:///$(pwd -W)/dev.db"
```

Abra o endereço que aparecer no terminal (por exemplo, `http://localhost:51212`).
Pare com `Ctrl + C`.

## Problemas comuns

| Erro | Solução |
|---|---|
| `Environment variable not found: DATABASE_URL` | Faltou o passo 3 (`cp .env.example .env`). |
| `Cannot find type definition file for 'node'` | Rode `npm install` de novo. |
| Porta 3000 em uso | Pare o outro servidor com `Ctrl + C`, ou use outra porta: `PORT=3001 npm run start:dev`. |
| `Cannot find module ... generated/prisma` | Rode `npx prisma generate`. |
| `GET /produto` volta `[]` | O banco está vazio. Crie uma categoria e depois os produtos. |

## Estrutura

```
src/
  produto/      controller, service, DTOs e módulo de produtos
  categoria/    controller, service, DTOs e módulo de categorias
  prisma/       PrismaService (conexão com o banco)
prisma/
  schema.prisma modelos Categoria e Produto
  migrations/   histórico de alterações do banco
bruno/          coleção de requisições para o Bruno
```