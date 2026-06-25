# FastAPI MCP Todo

A simple Todo List API built with **FastAPI** and **SQLite**, exposed as both a REST API and an **MCP (Model Context Protocol)** server. AI clients such as Cursor can manage todos through MCP tools while humans can use the standard HTTP endpoints or the interactive docs.

## Description

This project is a lightweight backend for managing a personal todo list. It provides full CRUD operations (create, read, update, delete) over HTTP and automatically exposes the same operations as MCP tools via [fastapi-mcp](https://github.com/tadata-org/fastapi-mcp).

The API persists data in a local SQLite database (`todos.db`) and ships with OpenAPI documentation at `/docs`. The MCP endpoint is mounted at `/mcp` using Streamable HTTP transport, so compatible clients can list, create, update, and delete todos without writing raw HTTP calls.

## Features

- REST API with automatic OpenAPI / Swagger UI at `/docs`
- SQLite persistence (no external database required)
- MCP server integration for AI-assisted todo management
- Deploy-ready configuration for [Render](https://render.com) via `render.yaml`

## Tech Stack

- [FastAPI](https://fastapi.tiangolo.com/)
- [SQLite](https://www.sqlite.org/)
- [fastapi-mcp](https://pypi.org/project/fastapi-mcp/)
- [Uvicorn](https://www.uvicorn.org/)

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/` | Welcome message |
| `GET` | `/todos` | List all todos |
| `GET` | `/todos/{id}` | Get a single todo |
| `POST` | `/todos` | Create a todo |
| `PATCH` | `/todos/{id}` | Update a todo (content and/or completed) |
| `DELETE` | `/todos/{id}` | Delete a todo |

### Todo model

```json
{
  "id": 1,
  "content": "Buy groceries",
  "completed": false
}
```

## MCP Tools

When the MCP server is mounted, the following tools are available:

| Tool | Description |
|------|-------------|
| `get_all_todos` | Retrieve all todos |
| `get_todo` | Fetch a todo by ID |
| `create_todo` | Add a new todo |
| `update_todo` | Update content or completed status |
| `delete_todo` | Remove a todo |

MCP endpoint: `GET/POST /mcp` (Streamable HTTP)

## Getting Started

### Prerequisites

- Python 3.12+
- pip

### Installation

```bash
git clone <repository-url>
cd fast-api-mcp-todo
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### Run locally

```bash
uvicorn main:app --reload
```

The API will be available at:

- API: http://127.0.0.1:8000
- Docs: http://127.0.0.1:8000/docs
- MCP: http://127.0.0.1:8000/mcp

### Example requests

```bash
# Create a todo
curl -X POST http://127.0.0.1:8000/todos \
  -H "Content-Type: application/json" \
  -d '{"content": "Learn FastAPI"}'

# List todos
curl http://127.0.0.1:8000/todos

# Mark as completed
curl -X PATCH http://127.0.0.1:8000/todos/1 \
  -H "Content-Type: application/json" \
  -d '{"completed": true}'
```

## Deployment

The project includes a `render.yaml` blueprint for deploying to Render as a free-tier web service. Render runs:

```bash
uvicorn main:app --host 0.0.0.0 --port $PORT
```

After deployment, point your MCP client at `https://<your-service>.com/mcp`. In my case this is the url: https://fastapi-mcp-todo-45ud.onrender.com

## Project Structure

```
fast-api-mcp-todo/
├── main.py           # FastAPI app, routes, and MCP setup
├── requirements.txt  # Python dependencies
├── render.yaml       # Render deployment config
├── todos.db          # SQLite database (created on first run)
└── README.md
```

## License

MIT
