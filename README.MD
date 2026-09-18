# Level Up Games - API de Catálogo de Jogos

API em PHP para centralizar o catálogo de jogos da Level Up Games, unificando os produtos vendidos pelo site e pelo Instagram em uma única fonte de dados — evitando vender itens que já estão esgotados.

## 📋 Sobre o projeto

A Level Up Games vendia pelos dois canais (site e Instagram) usando listas separadas, o que causava divergência de estoque e venda de jogos esgotados. Esta API resolve isso oferecendo:

- **Cadastro** de jogos no catálogo (via `POST`)
- **Listagem** de todos os jogos, em ordem alfabética pelo título (via `GET`)

A comunicação é feita em formato **JSON**.

## 🗂️ Estrutura do projeto

```
├── conexao.php     # Configurações de conexão com o banco (NÃO versionado)
└── jogos.php       # Rotas de cadastro (POST) e listagem (GET) de jogos
```

> ⚠️ O arquivo `conexao.php` contém informações sensíveis (host, usuário, senha) e por isso está incluído no `.gitignore`, não sendo enviado ao repositório.

## ⚙️ Tecnologias utilizadas

- PHP (nativo, sem frameworks)
- PDO (PHP Data Objects) para acesso ao banco de dados
- PostgreSQL

## 🔌 Como funciona

O arquivo `jogos.php` verifica o método HTTP da requisição e executa a ação correspondente:

### `POST` - Cadastrar jogo

Recebe um JSON no corpo da requisição com os dados do jogo e insere um novo registro na tabela `jogos`.

**Exemplo de requisição:**
```json
{
  "titulo": "Minecraft",
  "plataforma": "PC",
  "genero": "Sandbox",
  "desenvolvedora": "Mojang",
  "ano_lancamento": 2011,
  "preco": 99.90,
  "estoque": 25
}
```

> Em JSON, número decimal usa ponto (`99.90`), não vírgula.

**Resposta:**
```json
{
  "Mensagem": "Jogo cadastrado com sucesso!!!"
}
```

### `GET` - Listar jogos

Retorna todos os jogos cadastrados, em formato JSON, ordenados alfabeticamente por `titulo`.

**Resposta:**
```json
[
  {
    "id": 1,
    "titulo": "Minecraft",
    "plataforma": "PC",
    "genero": "Sandbox",
    "desenvolvedora": "Mojang",
    "ano_lancamento": 2011,
    "preco": "99.90",
    "estoque": 25
  }
]
```

## 🛠️ Configuração do banco de dados

Crie o banco `levelup` no PostgreSQL e a tabela `jogos`:

```sql
CREATE DATABASE levelup;

CREATE TABLE jogos(
    id SERIAL PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL,
    plataforma VARCHAR(70) NOT NULL,
    genero VARCHAR(70) NOT NULL,
    desenvolvedora VARCHAR(100) NOT NULL,
    ano_lancamento INT NOT NULL,
    preco NUMERIC(10,2) NOT NULL,
    estoque INT NOT NULL
);
```

Crie um arquivo `conexao.php` na raiz do projeto com o seguinte conteúdo (ajuste conforme seu ambiente):

```php
<?php
$host = "seu_host";
$usuario = "seu_usuario";
$banco = "levelup";
$senha = "sua_senha";

$pdo = new PDO(
    "pgsql:host=$host;port=5432;dbname=$banco",
    $usuario,
    $senha
);
?>
```

## ▶️ Como rodar o projeto

1. Clone o repositório
2. Crie o banco `levelup` e a tabela `jogos` (veja seção acima)
3. Crie o arquivo `conexao.php` com suas credenciais
4. Inicie um servidor local, por exemplo:
   ```bash
   php -S localhost:8000
   ```
5. Faça as requisições para `http://localhost:8000/jogos.php`

## 🧪 Testes

Cadastre alguns jogos via `POST` para validar o funcionamento, por exemplo os 5 abaixo:

- Grand Theft Auto V (PC, Ação/Aventura, Rockstar Games, 2013)
- The Legend of Zelda: Breath of the Wild (Switch, Aventura, Nintendo, 2017)
- God of War (PS4, Ação/Aventura, Santa Monica Studio, 2018)
- Elden Ring (PC, RPG, FromSoftware, 2022)
- Stardew Valley (PC, Simulação, ConcernedApe, 2016)

Depois, use o `GET` para conferir se todos aparecem listados em ordem alfabética.

## 📌 Próximos passos (ideias de melhoria)

- Adicionar validação dos dados recebidos (tipos, campos obrigatórios)
- Implementar métodos `PUT` (atualizar) e `DELETE` (remover)
- Adicionar filtro por plataforma ou gênero na listagem
- Criar autenticação para proteger as rotas de cadastro

## 📄 Licença

Este projeto está sob a licença MIT. Sinta-se livre para usar e modificar.