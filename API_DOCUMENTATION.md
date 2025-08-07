# NexusAI API Documentation

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Backend APIs](#backend-apis)
   - [Authentication API](#authentication-api)
   - [Agent API](#agent-api)
   - [Workflow API](#workflow-api)
   - [Vector API](#vector-api)
   - [Upload API](#upload-api)
   - [Skill API](#skill-api)
   - [Tools API](#tools-api)
   - [LLM API](#llm-api)
   - [Workspace API](#workspace-api)
   - [Chatroom API](#chatroom-api)
   - [Apps API](#apps-api)
   - [MCP Servers API](#mcp-servers-api)
   - [Roles API](#roles-api)
   - [Prompt Editor API](#prompt-editor-api)
   - [Supplier API](#supplier-api)
   - [Icon API](#icon-api)
   - [Tags API](#tags-api)
4. [Frontend Components](#frontend-components)
   - [Core Components](#core-components)
   - [Workflow Components](#workflow-components)
   - [Chat Components](#chat-components)
   - [Agent Components](#agent-components)
   - [Utility Components](#utility-components)
5. [Core Modules](#core-modules)
6. [Configuration](#configuration)
7. [Examples](#examples)

## Overview

NexusAI is an open-source platform that fuses human and AI capabilities for task automation, intelligent decision-making, and full-process visualization. The platform consists of:

- **Backend**: FastAPI-based REST API with Python
- **Frontend**: React-based web application with Ant Design Pro
- **Core Modules**: Python modules for AI/ML functionality
- **Database**: MySQL with SQLAlchemy ORM
- **Vector Database**: Milvus for embeddings
- **Message Queue**: Redis and Celery for async tasks

## Authentication

NexusAI uses OAuth2 with password flow for authentication. All API endpoints require authentication unless explicitly marked as public.

### Authentication Flow

```python
# Login endpoint
POST /v1/auth/login
{
    "username": "your_username",
    "password": "your_password"
}

# Response includes access token
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "token_type": "bearer"
}
```

### Using Authentication

Include the access token in the Authorization header:

```bash
curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     https://your-nexusai-instance.com/v1/agent/list
```

## Backend APIs

### Authentication API

**Base Path**: `/v1/auth`

#### Endpoints

##### POST /v1/auth/login
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
    "access_token": "string",
    "token_type": "bearer",
    "user": {
        "id": "integer",
        "username": "string",
        "email": "string",
        "role": "string"
    }
}
```

##### POST /v1/auth/register
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

##### GET /v1/auth/profile
Get current user profile information.

**Response**:
```json
{
    "id": "integer",
    "username": "string",
    "email": "string",
    "role": "string",
    "created_at": "datetime",
    "updated_at": "datetime"
}
```

##### PUT /v1/auth/profile
Update user profile information.

**Request Body**:
```json
{
    "username": "string",
    "email": "string"
}
```

### Agent API

**Base Path**: `/v1/agent`

#### Endpoints

##### GET /v1/agent/list
Get list of all agents.

**Query Parameters**:
- `page`: Page number (default: 1)
- `size`: Page size (default: 10)
- `search`: Search term for agent name/description

**Response**:
```json
{
    "total": "integer",
    "items": [
        {
            "id": "integer",
            "name": "string",
            "description": "string",
            "avatar": "string",
            "model": "string",
            "temperature": "float",
            "max_tokens": "integer",
            "created_at": "datetime",
            "updated_at": "datetime"
        }
    ]
}
```

##### POST /v1/agent/create
Create a new agent.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "avatar": "string",
    "model": "string",
    "temperature": "float",
    "max_tokens": "integer",
    "prompt": "string",
    "tools": ["array of tool IDs"],
    "skills": ["array of skill IDs"]
}
```

##### GET /v1/agent/{agent_id}
Get agent details by ID.

**Response**:
```json
{
    "id": "integer",
    "name": "string",
    "description": "string",
    "avatar": "string",
    "model": "string",
    "temperature": "float",
    "max_tokens": "integer",
    "prompt": "string",
    "tools": ["array of tool objects"],
    "skills": ["array of skill objects"],
    "created_at": "datetime",
    "updated_at": "datetime"
}
```

##### PUT /v1/agent/{agent_id}
Update agent information.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "avatar": "string",
    "model": "string",
    "temperature": "float",
    "max_tokens": "integer",
    "prompt": "string",
    "tools": ["array of tool IDs"],
    "skills": ["array of skill IDs"]
}
```

##### DELETE /v1/agent/{agent_id}
Delete an agent.

##### POST /v1/agent/{agent_id}/chat
Chat with an agent.

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
    "response": "string",
    "conversation_id": "string",
    "tokens_used": "integer",
    "execution_time": "float"
}
```

### Workflow API

**Base Path**: `/v1/workflow`

#### Endpoints

##### GET /v1/workflow/list
Get list of all workflows.

**Query Parameters**:
- `page`: Page number (default: 1)
- `size`: Page size (default: 10)
- `search`: Search term for workflow name/description

**Response**:
```json
{
    "total": "integer",
    "items": [
        {
            "id": "integer",
            "name": "string",
            "description": "string",
            "nodes": ["array of node objects"],
            "edges": ["array of edge objects"],
            "created_at": "datetime",
            "updated_at": "datetime"
        }
    ]
}
```

##### POST /v1/workflow/create
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
            "type": "string"
        }
    ]
}
```

##### GET /v1/workflow/{workflow_id}
Get workflow details by ID.

##### PUT /v1/workflow/{workflow_id}
Update workflow.

##### DELETE /v1/workflow/{workflow_id}
Delete a workflow.

##### POST /v1/workflow/{workflow_id}/execute
Execute a workflow.

**Request Body**:
```json
{
    "input_data": "object",
    "parameters": "object"
}
```

**Response**:
```json
{
    "execution_id": "string",
    "status": "string",
    "result": "object",
    "execution_time": "float"
}
```

### Vector API

**Base Path**: `/v1/vector`

#### Endpoints

##### POST /v1/vector/upload
Upload documents for vectorization.

**Request Body** (multipart/form-data):
- `file`: Document file
- `collection_name`: Vector collection name
- `metadata`: Additional metadata (optional)

**Response**:
```json
{
    "collection_id": "string",
    "document_id": "string",
    "status": "string",
    "chunks": "integer"
}
```

##### POST /v1/vector/search
Search vector database.

**Request Body**:
```json
{
    "query": "string",
    "collection_name": "string",
    "top_k": "integer",
    "filter": "object (optional)"
}
```

**Response**:
```json
{
    "results": [
        {
            "id": "string",
            "content": "string",
            "metadata": "object",
            "score": "float"
        }
    ]
}
```

##### GET /v1/vector/collections
Get list of vector collections.

**Response**:
```json
{
    "collections": [
        {
            "name": "string",
            "document_count": "integer",
            "vector_count": "integer",
            "created_at": "datetime"
        }
    ]
}
```

### Upload API

**Base Path**: `/v1/upload`

#### Endpoints

##### POST /v1/upload/file
Upload a file.

**Request Body** (multipart/form-data):
- `file`: File to upload
- `type`: File type (document, image, etc.)

**Response**:
```json
{
    "file_id": "string",
    "filename": "string",
    "size": "integer",
    "url": "string",
    "type": "string"
}
```

##### GET /v1/upload/files
Get list of uploaded files.

**Query Parameters**:
- `page`: Page number (default: 1)
- `size`: Page size (default: 10)
- `type`: Filter by file type

##### DELETE /v1/upload/file/{file_id}
Delete an uploaded file.

### Skill API

**Base Path**: `/v1/skill`

#### Endpoints

##### GET /v1/skill/list
Get list of all skills.

**Response**:
```json
{
    "total": "integer",
    "items": [
        {
            "id": "integer",
            "name": "string",
            "description": "string",
            "code": "string",
            "parameters": "object",
            "created_at": "datetime"
        }
    ]
}
```

##### POST /v1/skill/create
Create a new skill.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "code": "string",
    "parameters": "object"
}
```

##### GET /v1/skill/{skill_id}
Get skill details by ID.

##### PUT /v1/skill/{skill_id}
Update a skill.

##### DELETE /v1/skill/{skill_id}
Delete a skill.

### Tools API

**Base Path**: `/v1/tool`

#### Endpoints

##### GET /v1/tool/list
Get list of all tools.

**Response**:
```json
{
    "total": "integer",
    "items": [
        {
            "id": "integer",
            "name": "string",
            "description": "string",
            "type": "string",
            "parameters": "object",
            "created_at": "datetime"
        }
    ]
}
```

##### POST /v1/tool/create
Create a new tool.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "type": "string",
    "parameters": "object",
    "implementation": "string"
}
```

### LLM API

**Base Path**: `/v1/llm`

#### Endpoints

##### POST /v1/llm/chat
Send a chat message to an LLM.

**Request Body**:
```json
{
    "model": "string",
    "messages": [
        {
            "role": "string",
            "content": "string"
        }
    ],
    "temperature": "float",
    "max_tokens": "integer"
}
```

**Response**:
```json
{
    "response": "string",
    "tokens_used": "integer",
    "model": "string"
}
```

##### GET /v1/llm/models
Get available LLM models.

**Response**:
```json
{
    "models": [
        {
            "id": "string",
            "name": "string",
            "provider": "string",
            "max_tokens": "integer",
            "supports_functions": "boolean"
        }
    ]
}
```

### Workspace API

**Base Path**: `/v1/workspace`

#### Endpoints

##### GET /v1/workspace/list
Get list of workspaces.

##### POST /v1/workspace/create
Create a new workspace.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "settings": "object"
}
```

##### GET /v1/workspace/{workspace_id}
Get workspace details.

##### PUT /v1/workspace/{workspace_id}
Update workspace.

##### DELETE /v1/workspace/{workspace_id}
Delete a workspace.

### Chatroom API

**Base Path**: `/v1/chatroom`

#### Endpoints

##### GET /v1/chatroom/list
Get list of chatrooms.

##### POST /v1/chatroom/create
Create a new chatroom.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "agents": ["array of agent IDs"],
    "settings": "object"
}
```

##### GET /v1/chatroom/{chatroom_id}
Get chatroom details.

##### POST /v1/chatroom/{chatroom_id}/message
Send a message to chatroom.

**Request Body**:
```json
{
    "message": "string",
    "sender_type": "string (user|agent)"
}
```

##### GET /v1/chatroom/{chatroom_id}/messages
Get chatroom messages.

**Query Parameters**:
- `page`: Page number (default: 1)
- `size`: Page size (default: 50)

### Apps API

**Base Path**: `/v1/apps`

#### Endpoints

##### GET /v1/apps/list
Get list of applications.

##### POST /v1/apps/create
Create a new application.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "type": "string",
    "config": "object"
}
```

##### GET /v1/apps/{app_id}
Get application details.

##### PUT /v1/apps/{app_id}
Update application.

##### DELETE /v1/apps/{app_id}
Delete an application.

### MCP Servers API

**Base Path**: `/v1/mcp-servers`

#### Endpoints

##### GET /v1/mcp-servers/list
Get list of MCP servers.

##### POST /v1/mcp-servers/create
Create a new MCP server.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "endpoint": "string",
    "authentication": "object"
}
```

### Roles API

**Base Path**: `/v1/roles`

#### Endpoints

##### GET /v1/roles/list
Get list of roles.

##### POST /v1/roles/create
Create a new role.

**Request Body**:
```json
{
    "name": "string",
    "description": "string",
    "permissions": ["array of permissions"]
}
```

### Prompt Editor API

**Base Path**: `/prompt-editor`

#### Endpoints

##### GET /prompt-editor/templates
Get prompt templates.

##### POST /prompt-editor/template
Create a new prompt template.

**Request Body**:
```json
{
    "name": "string",
    "content": "string",
    "variables": ["array of variable names"]
}
```

### Supplier API

**Base Path**: `/v1/supplier`

#### Endpoints

##### GET /v1/supplier/list
Get list of suppliers.

##### POST /v1/supplier/create
Create a new supplier.

**Request Body**:
```json
{
    "name": "string",
    "api_key": "string",
    "endpoint": "string",
    "models": ["array of model names"]
}
```

### Icon API

**Base Path**: `/v1/icon`

#### Endpoints

##### GET /v1/icon/list
Get list of available icons.

##### POST /v1/icon/upload
Upload a custom icon.

### Tags API

**Base Path**: `/v1/tag`

#### Endpoints

##### GET /v1/tag/list
Get list of tags.

##### POST /v1/tag/create
Create a new tag.

**Request Body**:
```json
{
    "name": "string",
    "color": "string",
    "description": "string"
}
```

## Frontend Components

### Core Components

#### ModelSetup.tsx
Component for configuring AI models and settings.

**Props**:
```typescript
interface ModelSetupProps {
    visible: boolean;
    onCancel: () => void;
    onOk: (config: ModelConfig) => void;
    initialConfig?: ModelConfig;
}
```

**Usage**:
```tsx
import ModelSetup from '@/components/ModelSetup';

<ModelSetup
    visible={isVisible}
    onCancel={() => setIsVisible(false)}
    onOk={(config) => {
        console.log('Model config:', config);
        setIsVisible(false);
    }}
/>
```

#### CreationModal.tsx
Generic modal component for creating new resources.

**Props**:
```typescript
interface CreationModalProps {
    visible: boolean;
    title: string;
    onCancel: () => void;
    onOk: (data: any) => void;
    formItems: FormItem[];
    initialValues?: any;
}
```

#### TagSearch.tsx
Component for searching and selecting tags.

**Props**:
```typescript
interface TagSearchProps {
    value?: string[];
    onChange: (tags: string[]) => void;
    placeholder?: string;
    maxTags?: number;
}
```

### Workflow Components

#### WorkFlow/
Directory containing workflow-related components.

**Key Components**:
- `WorkflowCanvas.tsx`: Main workflow editor canvas
- `WorkflowNode.tsx`: Individual workflow node component
- `WorkflowToolbar.tsx`: Workflow editing toolbar
- `NodeTypes/`: Different types of workflow nodes

**Usage**:
```tsx
import WorkflowCanvas from '@/components/WorkFlow/WorkflowCanvas';

<WorkflowCanvas
    nodes={workflowNodes}
    edges={workflowEdges}
    onNodesChange={handleNodesChange}
    onEdgesChange={handleEdgesChange}
    onConnect={handleConnect}
/>
```

### Chat Components

#### ChatRoom/
Directory containing chat-related components.

**Key Components**:
- `ChatRoom.tsx`: Main chat room component
- `ChatMessage.tsx`: Individual chat message component
- `ChatInput.tsx`: Chat input component
- `ChatAvatar.tsx`: User/agent avatar component

**Usage**:
```tsx
import ChatRoom from '@/components/ChatRoom/ChatRoom';

<ChatRoom
    messages={messages}
    onSendMessage={handleSendMessage}
    agents={agents}
    isLoading={isLoading}
/>
```

#### ChatRoomContent.tsx
Component for displaying chat room content.

**Props**:
```typescript
interface ChatRoomContentProps {
    messages: ChatMessage[];
    isLoading: boolean;
    onLoadMore: () => void;
    hasMore: boolean;
}
```

#### ChatRoomAvatar.tsx
Component for displaying chat room avatars.

**Props**:
```typescript
interface ChatRoomAvatarProps {
    avatar: string;
    name: string;
    size?: number;
    shape?: 'circle' | 'square';
}
```

### Agent Components

#### AgentCreate/
Directory containing agent creation and management components.

**Key Components**:
- `AgentForm.tsx`: Form for creating/editing agents
- `AgentList.tsx`: List of agents
- `AgentCard.tsx`: Individual agent card component

**Usage**:
```tsx
import AgentForm from '@/components/AgentCreate/AgentForm';

<AgentForm
    initialValues={agentData}
    onSubmit={handleSubmit}
    onCancel={handleCancel}
/>
```

### Utility Components

#### InfiniteScroll.tsx
Component for infinite scrolling lists.

**Props**:
```typescript
interface InfiniteScrollProps {
    dataLength: number;
    next: () => void;
    hasMore: boolean;
    children: React.ReactNode;
    loader?: React.ReactNode;
}
```

**Usage**:
```tsx
import InfiniteScroll from '@/components/InfiniteScroll';

<InfiniteScroll
    dataLength={items.length}
    next={loadMoreItems}
    hasMore={hasMore}
    loader={<div>Loading...</div>}
>
    {items.map(item => (
        <div key={item.id}>{item.name}</div>
    ))}
</InfiniteScroll>
```

#### DivEditable.tsx
Editable div component.

**Props**:
```typescript
interface DivEditableProps {
    value: string;
    onChange: (value: string) => void;
    placeholder?: string;
    disabled?: boolean;
    multiline?: boolean;
}
```

#### FileListDisplay.tsx
Component for displaying file lists.

**Props**:
```typescript
interface FileListDisplayProps {
    files: FileItem[];
    onDelete?: (fileId: string) => void;
    onDownload?: (fileId: string) => void;
    showActions?: boolean;
}
```

#### FileUploadExample.tsx
Component for file upload functionality.

**Props**:
```typescript
interface FileUploadExampleProps {
    onUpload: (files: File[]) => void;
    accept?: string;
    multiple?: boolean;
    maxSize?: number;
}
```

## Core Modules

### Core Database Module

**Location**: `core/database/`

**Key Functions**:
- Database connection management
- Model definitions
- Migration utilities

**Usage**:
```python
from core.database import get_db, Base
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True, index=True)
    username = Column(String, unique=True, index=True)
```

### Core LLM Module

**Location**: `core/llm/`

**Key Functions**:
- LLM model management
- Chat completion
- Function calling

**Usage**:
```python
from core.llm import LLMManager

llm_manager = LLMManager()
response = await llm_manager.chat_completion(
    model="gpt-4",
    messages=[{"role": "user", "content": "Hello"}]
)
```

### Core Embeddings Module

**Location**: `core/embeddings/`

**Key Functions**:
- Text embedding generation
- Embedding model management
- Vector operations

**Usage**:
```python
from core.embeddings import EmbeddingManager

embedding_manager = EmbeddingManager()
embeddings = await embedding_manager.get_embeddings(["Hello world"])
```

### Core Vector Database Module

**Location**: `core/vdb/`

**Key Functions**:
- Vector database operations
- Collection management
- Similarity search

**Usage**:
```python
from core.vdb import VectorDB

vdb = VectorDB()
results = await vdb.search(
    collection_name="documents",
    query_vector=embeddings[0],
    top_k=10
)
```

### Core Tool Module

**Location**: `core/tool/`

**Key Functions**:
- Tool execution
- Tool registry
- Parameter validation

**Usage**:
```python
from core.tool import ToolManager

tool_manager = ToolManager()
result = await tool_manager.execute_tool(
    tool_name="web_search",
    parameters={"query": "NexusAI"}
)
```

### Core Workflow Module

**Location**: `core/workflow/`

**Key Functions**:
- Workflow execution
- Node processing
- Flow control

**Usage**:
```python
from core.workflow import WorkflowEngine

workflow_engine = WorkflowEngine()
result = await workflow_engine.execute_workflow(
    workflow_id="workflow_123",
    input_data={"text": "Hello"}
)
```

## Configuration

### Environment Variables

**Database Configuration**:
```bash
DATABASE_URL=mysql+pymysql://user:password@localhost/nexusai
```

**Redis Configuration**:
```bash
REDIS_URL=redis://localhost:6379
```

**Milvus Configuration**:
```bash
MILVUS_HOST=localhost
MILVUS_PORT=19530
```

**LLM API Keys**:
```bash
OPENAI_API_KEY=your_openai_api_key
ANTHROPIC_API_KEY=your_anthropic_api_key
```

**File Storage**:
```bash
STORAGE_PATH=/path/to/storage
UPLOAD_PATH=/path/to/uploads
```

### Configuration File

**Location**: `config.py`

**Key Settings**:
```python
class Settings:
    # API Configuration
    API_HOST: str = "0.0.0.0"
    API_PORT: int = 8000
    FASTAPI_WORKERS: int = 1
    
    # Database Configuration
    DATABASE_URL: str
    
    # Redis Configuration
    REDIS_URL: str
    
    # Milvus Configuration
    MILVUS_HOST: str
    MILVUS_PORT: int
    
    # LLM Configuration
    OPENAI_API_KEY: str
    ANTHROPIC_API_KEY: str
    
    # File Storage
    STORAGE_PATH: str
    UPLOAD_PATH: str
```

## Examples

### Creating an Agent

```python
import requests

# Authenticate
response = requests.post("http://localhost:8000/v1/auth/login", json={
    "username": "admin",
    "password": "password"
})
token = response.json()["access_token"]

# Create agent
headers = {"Authorization": f"Bearer {token}"}
agent_data = {
    "name": "Research Assistant",
    "description": "An AI agent for research tasks",
    "model": "gpt-4",
    "temperature": 0.7,
    "max_tokens": 2000,
    "prompt": "You are a helpful research assistant."
}

response = requests.post(
    "http://localhost:8000/v1/agent/create",
    json=agent_data,
    headers=headers
)
agent_id = response.json()["id"]
```

### Creating a Workflow

```python
workflow_data = {
    "name": "Document Analysis",
    "description": "Analyze uploaded documents",
    "nodes": [
        {
            "id": "node1",
            "type": "input",
            "position": {"x": 100, "y": 100},
            "data": {"label": "Document Input"}
        },
        {
            "id": "node2",
            "type": "llm",
            "position": {"x": 300, "y": 100},
            "data": {"label": "Analysis", "model": "gpt-4"}
        },
        {
            "id": "node3",
            "type": "output",
            "position": {"x": 500, "y": 100},
            "data": {"label": "Results"}
        }
    ],
    "edges": [
        {
            "id": "edge1",
            "source": "node1",
            "target": "node2"
        },
        {
            "id": "edge2",
            "source": "node2",
            "target": "node3"
        }
    ]
}

response = requests.post(
    "http://localhost:8000/v1/workflow/create",
    json=workflow_data,
    headers=headers
)
```

### Chatting with an Agent

```python
chat_data = {
    "message": "What is the capital of France?",
    "conversation_id": None
}

response = requests.post(
    f"http://localhost:8000/v1/agent/{agent_id}/chat",
    json=chat_data,
    headers=headers
)

print(response.json()["response"])
```

### Uploading Documents for Vector Search

```python
with open("document.pdf", "rb") as f:
    files = {"file": f}
    data = {"collection_name": "documents"}
    
    response = requests.post(
        "http://localhost:8000/v1/vector/upload",
        files=files,
        data=data,
        headers=headers
    )

# Search the documents
search_data = {
    "query": "What is machine learning?",
    "collection_name": "documents",
    "top_k": 5
}

response = requests.post(
    "http://localhost:8000/v1/vector/search",
    json=search_data,
    headers=headers
)

results = response.json()["results"]
for result in results:
    print(f"Score: {result['score']}")
    print(f"Content: {result['content']}")
    print("---")
```

### Frontend Component Usage

```tsx
import React, { useState } from 'react';
import { AgentForm } from '@/components/AgentCreate';
import { WorkflowCanvas } from '@/components/WorkFlow';
import { ChatRoom } from '@/components/ChatRoom';

const App = () => {
    const [agents, setAgents] = useState([]);
    const [workflows, setWorkflows] = useState([]);
    const [messages, setMessages] = useState([]);

    const handleCreateAgent = (agentData) => {
        // API call to create agent
        console.log('Creating agent:', agentData);
    };

    const handleCreateWorkflow = (workflowData) => {
        // API call to create workflow
        console.log('Creating workflow:', workflowData);
    };

    const handleSendMessage = (message) => {
        // API call to send message
        console.log('Sending message:', message);
    };

    return (
        <div>
            <AgentForm onSubmit={handleCreateAgent} />
            <WorkflowCanvas
                nodes={workflows.nodes}
                edges={workflows.edges}
                onNodesChange={setWorkflows}
            />
            <ChatRoom
                messages={messages}
                onSendMessage={handleSendMessage}
            />
        </div>
    );
};

export default App;
```

## Error Handling

### Common HTTP Status Codes

- `200 OK`: Request successful
- `201 Created`: Resource created successfully
- `400 Bad Request`: Invalid request data
- `401 Unauthorized`: Authentication required
- `403 Forbidden`: Insufficient permissions
- `404 Not Found`: Resource not found
- `422 Unprocessable Entity`: Validation error
- `500 Internal Server Error`: Server error

### Error Response Format

```json
{
    "detail": "Error message",
    "error_code": "ERROR_CODE",
    "timestamp": "2024-01-01T00:00:00Z"
}
```

### Handling Errors in Frontend

```tsx
import { message } from 'antd';

const handleApiCall = async () => {
    try {
        const response = await api.createAgent(agentData);
        message.success('Agent created successfully');
    } catch (error) {
        if (error.response?.status === 401) {
            message.error('Please login again');
            // Redirect to login
        } else if (error.response?.status === 400) {
            message.error(error.response.data.detail);
        } else {
            message.error('An unexpected error occurred');
        }
    }
};
```

## Rate Limiting

API endpoints are rate-limited to prevent abuse. Limits are applied per user and per endpoint.

### Rate Limit Headers

- `X-RateLimit-Limit`: Maximum requests per window
- `X-RateLimit-Remaining`: Remaining requests in current window
- `X-RateLimit-Reset`: Time when the rate limit resets

### Handling Rate Limits

```tsx
const handleRateLimit = (response) => {
    const remaining = response.headers['x-ratelimit-remaining'];
    const reset = response.headers['x-ratelimit-reset'];
    
    if (remaining === '0') {
        message.warning(`Rate limit exceeded. Resets at ${new Date(reset * 1000)}`);
    }
};
```

## WebSocket Support

NexusAI supports WebSocket connections for real-time communication.

### WebSocket Endpoints

- `/ws/chat/{chatroom_id}`: Real-time chat
- `/ws/workflow/{workflow_id}`: Workflow execution updates
- `/ws/agent/{agent_id}`: Agent status updates

### WebSocket Usage

```javascript
const ws = new WebSocket('ws://localhost:8000/ws/chat/123');

ws.onopen = () => {
    console.log('WebSocket connected');
};

ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log('Received:', data);
};

ws.onclose = () => {
    console.log('WebSocket disconnected');
};
```

## Testing

### API Testing

```python
import pytest
from fastapi.testclient import TestClient
from app import app

client = TestClient(app)

def test_create_agent():
    response = client.post("/v1/agent/create", json={
        "name": "Test Agent",
        "description": "Test description",
        "model": "gpt-4"
    })
    assert response.status_code == 201
    assert response.json()["name"] == "Test Agent"
```

### Frontend Testing

```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import AgentForm from '@/components/AgentCreate/AgentForm';

test('creates agent when form is submitted', () => {
    const mockSubmit = jest.fn();
    render(<AgentForm onSubmit={mockSubmit} />);
    
    fireEvent.change(screen.getByLabelText('Name'), {
        target: { value: 'Test Agent' }
    });
    
    fireEvent.click(screen.getByText('Create'));
    
    expect(mockSubmit).toHaveBeenCalledWith({
        name: 'Test Agent'
    });
});
```

## Deployment

### Docker Deployment

```bash
# Build and run with Docker Compose
cd docker
docker-compose up -d
```

### Manual Deployment

```bash
# Install dependencies
pip install -r requirements.txt
npm install

# Set up database
alembic upgrade head

# Start services
python app.py
npm start
```

## Support

For support and questions:

- **Documentation**: [GitHub Wiki](https://github.com/EDEAI/NexusAI/wiki)
- **Issues**: [GitHub Issues](https://github.com/EDEAI/NexusAI/issues)
- **Discussions**: [GitHub Discussions](https://github.com/EDEAI/NexusAI/discussions)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.