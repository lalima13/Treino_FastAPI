# API REST FastAPI

Uma API REST moderna e robusta construída com **FastAPI**, com autenticação JWT e gerenciamento completo de posts. Este projeto demonstra boas práticas de desenvolvimento, incluindo arquitetura em camadas, testes integrados e suporte a múltiplos bancos de dados.

## 🚀 Características

- ✅ **Autenticação JWT**: Segurança integrada com tokens JWT
- ✅ **CRUD de Posts**: Criação, leitura, atualização e exclusão de posts
- ✅ **Arquitetura em Camadas**: Separação clara entre controllers, services, models e schemas
- ✅ **Suporte Multiplataforma**: Funciona com SQLite, PostgreSQL e AsyncPG
- ✅ **Testes Integrados**: Suite completa de testes com pytest
- ✅ **Banco de Dados Assíncrono**: Operações não-bloqueantes com databases
- ✅ **Validação de Dados**: Schemas Pydantic para validação automática

## 📋 Pré-requisitos

- Python >= 3.14
- pip ou Poetry
- SQLite ou PostgreSQL (opcional)

## 🛠️ Instalação

### Usando Poetry (Recomendado)

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/treino_fastapi.git
cd treino_fastapi

# Instale as dependências
poetry install

# Ative o ambiente virtual
poetry shell
```

### Usando pip

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/treino_fastapi.git
cd treino_fastapi

# Crie um ambiente virtual
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# ou
.venv\Scripts\activate  # Windows

# Instale as dependências
pip install fastapi uvicorn databases[aiosqlite,asyncpg] pyjwt psycopg2-binary
```

## 📚 Dependências Principais

| Pacote | Versão | Descrição |
|--------|--------|-----------|
| fastapi | latest | Framework web assíncrono |
| uvicorn | >=0.48.0 | Servidor ASGI |
| databases | latest | Acesso assíncrono a bancos de dados |
| pyjwt | >=2.13.0 | Implementação JWT |
| psycopg2-binary | >=2.9.12 | Driver PostgreSQL |

## 🚀 Como Executar

```bash
# Inicie o servidor de desenvolvimento
poetry run uvicorn src.main:app --reload

# Ou sem Poetry
uvicorn src.main:app --reload
```

O servidor será iniciado em `http://localhost:8000`

### Documentação Interativa

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## 📖 Estrutura do Projeto

```
treino_fastapi/
├── src/
│   ├── main.py                 # Aplicação principal e configuração
│   ├── database.py             # Configuração do banco de dados
│   ├── security.py             # Autenticação JWT
│   ├── controllers/            # Rotas e endpoints
│   │   ├── auth.py             # Autenticação
│   │   └── post.py             # Gerenciamento de posts
│   ├── models/                 # Modelos SQLAlchemy
│   │   └── post.py             # Modelo de Posts
│   ├── schemas/                # Schemas Pydantic
│   │   ├── auth.py             # Schemas de autenticação
│   │   └── post.py             # Schemas de posts
│   ├── services/               # Lógica de negócio
│   │   └── post.py             # Serviço de posts
│   └── views/                  # Response models
│       ├── auth.py             # Views de autenticação
│       └── post.py             # Views de posts
├── tests/
│   ├── conftest.py             # Configuração dos testes
│   └── integrations/
│       └── controllers/        # Testes integrados
│           ├── auth/
│           │   └── test_login.py
│           └── post/
│               ├── test_create_post.py
│               ├── test_read_post.py
│               ├── test_read_all.py
│               ├── test_update_post.py
│               └── test_delete_post.py
└── pyproject.toml              # Configuração do projeto
```

## 🔐 Autenticação

A API utiliza **JWT (JSON Web Tokens)** para autenticação. Todos os endpoints de posts requerem um token válido.

### Login

```bash
curl -X POST "http://localhost:8000/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"user_id": 1}'
```

**Resposta:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer"
}
```

### Usando o Token

Adicione o token no header `Authorization`:

```bash
curl -X GET "http://localhost:8000/posts/" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json"
```

## 📝 Endpoints Disponíveis

### Autenticação

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| POST | `/auth/login` | Gera um token JWT |

### Posts

| Método | Endpoint | Descrição | Autenticação |
|--------|----------|-----------|--------------|
| GET | `/posts/` | Lista todos os posts | ✅ |
| POST | `/posts/` | Cria um novo post | ✅ |
| GET | `/posts/{id}` | Obtém um post específico | ✅ |
| PATCH | `/posts/{id}` | Atualiza um post | ✅ |
| DELETE | `/posts/{id}` | Deleta um post | ✅ |

### Exemplos de Uso

#### Criar Post

```bash
curl -X POST "http://localhost:8000/posts/" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Meu Primeiro Post",
    "content": "Conteúdo do post",
    "published": false
  }'
```

#### Listar Posts

```bash
curl -X GET "http://localhost:8000/posts/?published=true&limit=10" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

#### Atualizar Post

```bash
curl -X PATCH "http://localhost:8000/posts/1" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Título Atualizado",
    "published": true
  }'
```

#### Deletar Post

```bash
curl -X DELETE "http://localhost:8000/posts/1" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## 🧪 Testes

Execute a suite de testes com pytest:

```bash
# Executar todos os testes
poetry run pytest

# Executar com saída verbosa
poetry run pytest -v

# Executar um arquivo específico
poetry run pytest tests/integrations/controllers/auth/test_login.py

# Executar com cobertura
poetry run pytest --cov=src
```

## 📊 Modelo de Dados

### Posts

```sql
CREATE TABLE posts (
    id INTEGER PRIMARY KEY,
    title VARCHAR(150) NOT NULL UNIQUE,
    content TEXT NOT NULL,
    published_at TIMESTAMP WITH TIME ZONE,
    published BOOLEAN DEFAULT FALSE
);
```

## 🗄️ Configuração do Banco de Dados

### SQLite (Padrão)

```python
DATABASE_URL = "sqlite:///./blog.db"
```

### PostgreSQL

```python
DATABASE_URL = "postgresql://user:password@localhost/dbname"
```

Edite a configuração em `src/database.py` conforme necessário.

## 🌳 Padrões de Arquitetura

O projeto segue a **arquitetura em camadas**:

1. **Controllers**: Recebem requisições HTTP e as delegam aos serviços
2. **Services**: Contêm a lógica de negócio
3. **Models**: Representam a estrutura dos dados no banco
4. **Schemas**: Validam e serializam dados (Pydantic)
5. **Views**: Modelos de resposta customizados
6. **Security**: Gerencia autenticação e autorização

## 🔧 Tecnologias Utilizadas

- **FastAPI**: Framework web de alto desempenho
- **Uvicorn**: Servidor ASGI assíncrono
- **SQLAlchemy**: ORM para gerenciamento de banco de dados
- **Pydantic**: Validação de dados
- **PyJWT**: Autenticação com JWT
- **pytest**: Framework de testes
- **asyncio**: Programação assíncrona

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

## 👤 Autor

**Lucas Lima**
- Email: lucaslimatech98@gmail.com
- GitHub: [@lalima13](https://github.com/lalima13)

## 🤝 Contribuindo

Contribuições são bem-vindas! Por favor:

1. Faça um Fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📮 Suporte

Para sugestões, dúvidas ou reportar bugs, abra uma [Issue](https://github.com/seu-usuario/treino_fastapi/issues).

---

**Made with ❤️ by Lucas Lima**
