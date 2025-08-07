# NexusAI API Reference

## Authentication

### POST /v1/auth/login
Authenticate user and get access token.

**Request Body**:
```json
{
    "username": "string",
    "password": "string"
}
```

**Response**:
```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "token_type": "bearer",
    "user": {
        "id": 1,
        "username": "admin",
        "email": "admin@example.com",
        "role": "admin"
    }
}
```

**Example**:
```bash
curl -X POST "http://localhost:8000/v1/auth/login" \
     -H "Content-Type: application/json" \
     -d '{"username": "admin", "password": "password"}'
```

### POST /v1/auth/register
Register a new user account.

**Request Body**:
```json
{
    "username": "string",
    "email": "string",
    "password": "string",
    "confirm_password": "string"
}
```

**Response**:
```json
{
    "id": 2,
    "username": "newuser",
    "email": "newuser@example.com",
    "role": "user",
    "created_at": "2024-01-01T00:00:00Z"
}
```

## Agent API

### GET /v1/agent/list
Get list of all agents.

**Query Parameters**:
- `page` (integer, optional): Page number (default: 1)
- `size` (integer, optional): Page size (default: 10)
- `search` (string, optional): Search term for agent name/description

**Response**:
```json
{
    "total": 25,
    "items": [
        {
            "id": 1,
            "name": "Research Assistant",
            "description": "An AI agent for research tasks",
            "avatar": "https://example.com/avatar1.png",
            "model": "gpt-4",
            "temperature": 0.7,
            "max_tokens": 2000,
            "created_at": "2024-01-01T00:00:00Z",
            "updated_at": "2024-01-01T00:00:00Z"
        }
    ]
}
```

### POST /v1/agent/create
Create a new agent.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "avatar": "string (optional)",
    "model": "string",
    "temperature": "float (optional, default: 0.7)",
    "max_tokens": "integer (optional, default: 2000)",
    "prompt": "string",
    "tools": ["array of tool IDs (optional)"],
    "skills": ["array of skill IDs (optional)"]
}
```

**Response**:
```json
{
    "id": 1,
    "name": "Research Assistant",
    "description": "An AI agent for research tasks",
    "avatar": "https://example.com/avatar1.png",
    "model": "gpt-4",
    "temperature": 0.7,
    "max_tokens": 2000,
    "prompt": "You are a helpful research assistant.",
    "tools": [],
    "skills": [],
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
}
```

### GET /v1/agent/{agent_id}
Get agent details by ID.

**Path Parameters**:
- `agent_id` (integer): Agent ID

**Response**:
```json
{
    "id": 1,
    "name": "Research Assistant",
    "description": "An AI agent for research tasks",
    "avatar": "https://example.com/avatar1.png",
    "model": "gpt-4",
    "temperature": 0.7,
    "max_tokens": 2000,
    "prompt": "You are a helpful research assistant.",
    "tools": [
        {
            "id": 1,
            "name": "Web Search",
            "description": "Search the web for information"
        }
    ],
    "skills": [
        {
            "id": 1,
            "name": "Data Analysis",
            "description": "Analyze data and generate insights"
        }
    ],
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
}
```

### POST /v1/agent/{agent_id}/chat
Chat with an agent.

**Path Parameters**:
- `agent_id` (integer): Agent ID

**Request Body**:
```json
{
    "message": "string",
    "conversation_id": "string (optional)",
    "context": "object (optional)"
}
```

**Response**:
```json
{
    "response": "The capital of France is Paris.",
    "conversation_id": "conv_123456",
    "tokens_used": 150,
    "execution_time": 2.5
}
```

## Workflow API

### GET /v1/workflow/list
Get list of all workflows.

**Query Parameters**:
- `page` (integer, optional): Page number (default: 1)
- `size` (integer, optional): Page size (default: 10)
- `search` (string, optional): Search term for workflow name/description

**Response**:
```json
{
    "total": 15,
    "items": [
        {
            "id": 1,
            "name": "Document Analysis",
            "description": "Analyze uploaded documents",
            "nodes": [
                {
                    "id": "node1",
                    "type": "input",
                    "position": {"x": 100, "y": 100},
                    "data": {"label": "Document Input"}
                }
            ],
            "edges": [
                {
                    "id": "edge1",
                    "source": "node1",
                    "target": "node2"
                }
            ],
            "created_at": "2024-01-01T00:00:00Z",
            "updated_at": "2024-01-01T00:00:00Z"
        }
    ]
}
```

### POST /v1/workflow/create
Create a new workflow.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "nodes": [
        {
            "id": "string",
            "type": "string",
            "position": {"x": "float", "y": "float"},
            "data": "object"
        }
    ],
    "edges": [
        {
            "id": "string",
            "source": "string",
            "target": "string",
            "type": "string (optional)"
        }
    ]
}
```

**Response**:
```json
{
    "id": 1,
    "name": "Document Analysis",
    "description": "Analyze uploaded documents",
    "nodes": [...],
    "edges": [...],
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
}
```

### POST /v1/workflow/{workflow_id}/execute
Execute a workflow.

**Path Parameters**:
- `workflow_id` (integer): Workflow ID

**Request Body**:
```json
{
    "input_data": "object",
    "parameters": "object (optional)"
}
```

**Response**:
```json
{
    "execution_id": "exec_123456",
    "status": "completed",
    "result": {
        "output": "Analysis completed successfully",
        "data": {...}
    },
    "execution_time": 15.5
}
```

## Vector API

### POST /v1/vector/upload
Upload documents for vectorization.

**Request Body** (multipart/form-data):
- `file` (file): Document file (PDF, DOCX, TXT, etc.)
- `collection_name` (string): Vector collection name
- `metadata` (string, optional): Additional metadata as JSON string

**Response**:
```json
{
    "collection_id": "col_123456",
    "document_id": "doc_789012",
    "status": "processing",
    "chunks": 15
}
```

### POST /v1/vector/search
Search vector database.

**Request Body**:
```json
{
    "query": "string",
    "collection_name": "string",
    "top_k": "integer (optional, default: 10)",
    "filter": "object (optional)"
}
```

**Response**:
```json
{
    "results": [
        {
            "id": "chunk_123",
            "content": "This is the relevant content...",
            "metadata": {
                "filename": "document.pdf",
                "page": 1,
                "chunk_id": 1
            },
            "score": 0.95
        }
    ]
}
```

## LLM API

### POST /v1/llm/chat
Send a chat message to an LLM.

**Request Body**:
```json
{
    "model": "string",
    "messages": [
        {
            "role": "string (user|assistant|system)",
            "content": "string"
        }
    ],
    "temperature": "float (optional, default: 0.7)",
    "max_tokens": "integer (optional, default: 2000)"
}
```

**Response**:
```json
{
    "response": "The response from the LLM",
    "tokens_used": 150,
    "model": "gpt-4"
}
```

### GET /v1/llm/models
Get available LLM models.

**Response**:
```json
{
    "models": [
        {
            "id": "gpt-4",
            "name": "GPT-4",
            "provider": "openai",
            "max_tokens": 8192,
            "supports_functions": true
        },
        {
            "id": "claude-3",
            "name": "Claude 3",
            "provider": "anthropic",
            "max_tokens": 100000,
            "supports_functions": true
        }
    ]
}
```

## Error Responses

### 400 Bad Request
```json
{
    "detail": "Invalid request data",
    "error_code": "VALIDATION_ERROR",
    "timestamp": "2024-01-01T00:00:00Z"
}
```

### 401 Unauthorized
```json
{
    "detail": "Authentication required",
    "error_code": "AUTH_REQUIRED",
    "timestamp": "2024-01-01T00:00:00Z"
}
```

### 404 Not Found
```json
{
    "detail": "Resource not found",
    "error_code": "NOT_FOUND",
    "timestamp": "2024-01-01T00:00:00Z"
}
```

### 422 Unprocessable Entity
```json
{
    "detail": [
        {
            "loc": ["body", "name"],
            "msg": "field required",
            "type": "value_error.missing"
        }
    ]
}
```

## Rate Limiting

API endpoints are rate-limited. Rate limit information is included in response headers:

- `X-RateLimit-Limit`: Maximum requests per window
- `X-RateLimit-Remaining`: Remaining requests in current window
- `X-RateLimit-Reset`: Time when the rate limit resets (Unix timestamp)

## WebSocket Endpoints

### /ws/chat/{chatroom_id}
Real-time chat WebSocket endpoint.

**Message Format**:
```json
{
    "type": "message|join|leave",
    "data": {
        "message": "string",
        "sender": "string",
        "timestamp": "2024-01-01T00:00:00Z"
    }
}
```

### /ws/workflow/{workflow_id}
Workflow execution updates WebSocket endpoint.

**Message Format**:
```json
{
    "type": "execution_update",
    "data": {
        "execution_id": "string",
        "status": "running|completed|failed",
        "progress": 75,
        "result": "object (optional)"
    }
}
```

## SDK Examples

### Python SDK
```python
import requests

class NexusAIClient:
    def __init__(self, base_url, token=None):
        self.base_url = base_url
        self.token = token
        self.session = requests.Session()
        if token:
            self.session.headers.update({"Authorization": f"Bearer {token}"})
    
    def login(self, username, password):
        response = self.session.post(f"{self.base_url}/v1/auth/login", json={
            "username": username,
            "password": password
        })
        data = response.json()
        self.token = data["access_token"]
        self.session.headers.update({"Authorization": f"Bearer {self.token}"})
        return data
    
    def create_agent(self, name, description, model, prompt):
        response = self.session.post(f"{self.base_url}/v1/agent/create", json={
            "name": name,
            "description": description,
            "model": model,
            "prompt": prompt
        })
        return response.json()
    
    def chat_with_agent(self, agent_id, message):
        response = self.session.post(f"{self.base_url}/v1/agent/{agent_id}/chat", json={
            "message": message
        })
        return response.json()

# Usage
client = NexusAIClient("http://localhost:8000")
client.login("admin", "password")
agent = client.create_agent("Helper", "A helpful agent", "gpt-4", "You are helpful.")
response = client.chat_with_agent(agent["id"], "Hello!")
```

### JavaScript SDK
```javascript
class NexusAIClient {
    constructor(baseUrl, token = null) {
        this.baseUrl = baseUrl;
        this.token = token;
    }
    
    async login(username, password) {
        const response = await fetch(`${this.baseUrl}/v1/auth/login`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ username, password })
        });
        const data = await response.json();
        this.token = data.access_token;
        return data;
    }
    
    async createAgent(name, description, model, prompt) {
        const response = await fetch(`${this.baseUrl}/v1/agent/create`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${this.token}`
            },
            body: JSON.stringify({ name, description, model, prompt })
        });
        return await response.json();
    }
    
    async chatWithAgent(agentId, message) {
        const response = await fetch(`${this.baseUrl}/v1/agent/${agentId}/chat`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${this.token}`
            },
            body: JSON.stringify({ message })
        });
        return await response.json();
    }
}

// Usage
const client = new NexusAIClient('http://localhost:8000');
await client.login('admin', 'password');
const agent = await client.createAgent('Helper', 'A helpful agent', 'gpt-4', 'You are helpful.');
const response = await client.chatWithAgent(agent.id, 'Hello!');
```