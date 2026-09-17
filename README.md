# Clientes.Api

API REST para cadastro e gestão de clientes, construída em **.NET 10** seguindo uma separação clara entre Controller, Service e Repository. Inclui testes automatizados, Swagger, Docker e pipeline de CI no GitHub Actions.

![.NET 10](https://img.shields.io/badge/.NET-10-512BD4?logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?logo=csharp&logoColor=white)
![EF Core](https://img.shields.io/badge/EF%20Core-SQL%20Server-CC2927?logo=microsoftsqlserver&logoColor=white)
![xUnit](https://img.shields.io/badge/Tests-xUnit%20%2B%20Moq-25A162)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)
[![CI](https://github.com/MaiconRB/Clientes.Api/actions/workflows/ci.yml/badge.svg)](https://github.com/MaiconRB/Clientes.Api/actions/workflows/ci.yml)

## Funcionalidades

CRUD completo de clientes via `api/clientes`:

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/api/clientes` | Lista todos os clientes |
| `GET` | `/api/clientes/{id}` | Busca um cliente pelo id |
| `POST` | `/api/clientes` | Cria um cliente |
| `PUT` | `/api/clientes/{id}` | Atualiza um cliente |
| `DELETE` | `/api/clientes/{id}` | Remove um cliente |

Cada cliente possui `Nome`, `Email` (validado) e `Documento`, com `Ativo` e `DataCadastro` controlados pela API.

## Arquitetura

```
Clientes.Api/
├── Controllers/    # Endpoints HTTP
├── Services/       # Regras de negócio (IClienteService)
├── Repositories/   # Acesso a dados (IClienteRepository)
├── Data/           # AppDbContext (EF Core)
├── DTOs/           # Contratos de entrada/saída
├── Models/         # Entidades de domínio
└── Migrations/     # Migrations do EF Core
```

A separação por interfaces (`IClienteService`, `IClienteRepository`) permite testar a camada de serviço isoladamente com mocks (Moq), sem depender do banco de dados.

## Rodando localmente

Pré-requisitos: [.NET 10 SDK](https://dotnet.microsoft.com/download) e um SQL Server acessível (local, container ou Azure).

```bash
# Ajuste a connection string "SqlServerConnection" em appsettings.json
dotnet restore
dotnet ef database update --project Clientes.Api
dotnet run --project Clientes.Api
```

A API sobe com o Swagger na raiz (`http://localhost:5053/`).

### Com Docker

```bash
docker build -t clientes-api -f Clientes.Api/Dockerfile Clientes.Api
docker run -p 8080:8080 clientes-api
```

## Testes

```bash
dotnet test
```

Os testes de `Clientes.Api.Tests` cobrem a camada de serviço com xUnit + Moq. O pipeline de CI (`.github/workflows/ci.yml`) roda build e testes a cada push/PR na `master`.
