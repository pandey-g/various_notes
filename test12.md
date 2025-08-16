### User Story: Define and Execute External Tools with Authentication, Schema, and Execution Logic

**Title**: Build a PoC to define external tools via UI with full backend support for authentication, input/output schema, and execution
**Priority**: High
**Type**: Feature
**Goal**: Enable non-technical users to configure and invoke third-party API tools securely and reliably using a no-code approach.

---

### Description

We want to build a standalone proof-of-concept backend module that allows users to define reusable "tools" that represent HTTP-based API functions. These tools should be configured through a UI and executed via a backend engine. This includes support for:

* Registering and managing tool metadata (e.g., endpoint, auth, schema)
* Calling external services dynamically
* Validating inputs and outputs using JSON Schema
* Managing authentication securely (OAuth2, bearer token, API key)
* Executing the tool with runtime inputs

This module will later integrate into a larger workflow engine or LLM orchestration layer, but must be independently functional.

---

### Acceptance Criteria

#### 1. Tool Registration

Users can register a tool with:

* `name` (string)
* `description` (optional text)
* `execution_type` (enum: `http_request`, `function`, etc.)
* `method` (e.g., GET, POST)
* `endpoint_url` (string)
* `headers` (key-value map)
* `timeout` (optional int)
* `auth_config` (object, see below)
* `input_schema` (JSON Schema v7)
* `output_schema` (JSON Schema v7)

#### 2. Authentication Support

Support the following auth types:

* **None** (no authentication)
* **Bearer Token** (attached to `Authorization` header)
* **API Key** (custom header or query param)
* **OAuth2 Client Credentials Flow**

Examples:

**Bearer Token**

```json
{
  "type": "bearer_token",
  "token": "xxxxx"
}
```

**API Key**

```json
{
  "type": "api_key",
  "key_name": "x-api-key",
  "location": "header",
  "value": "abc123"
}
```

**OAuth2**

```json
{
  "type": "oauth2",
  "token_url": "https://auth.example.com/oauth/token",
  "client_id": "my-client-id",
  "client_secret": "my-client-secret",
  "scope": "basic",
  "grant_type": "client_credentials"
}
```

#### 3. Schema Enforcement

* Validate incoming `input` using the tool’s `input_schema`
* Validate outgoing `output` from the API using the `output_schema`
* Return appropriate 422 or 500 errors if schema validation fails

#### 4. Tool Execution

* API endpoint like `POST /tools/{tool_id}/execute`
* Accepts JSON input
* Validates input against tool schema
* Injects inputs into the request (body, path, query, headers)
* Attaches appropriate auth
* Makes actual HTTP request
* Validates and returns parsed response

#### 5. Secret Management

* All sensitive values (`token`, `client_secret`, etc.) must be stored encrypted
* Secrets should **never be returned** in API responses
* Optional: store in vault or use symmetric encryption (e.g., Fernet)

---

###  Technical Design Suggestions

* Use `httpx.AsyncClient` to invoke HTTP requests
* Use `pydantic` and `jsonschema` for schema validation
* Encrypt secrets using `cryptography.fernet`
* Model suggestion:

  * `Tool`: stores general metadata
  * `AuthConfig`: polymorphic child model linked to Tool (use `auth_type` as discriminator)
* Optional: token caching for OAuth2 with expiry tracking
* Include tool usage logs for audit/debugging

---

### Test Cases

1. Register a simple webhook tool (no auth) and invoke it
2. Register a GET tool with API key in query params, test execution
3. Register a tool with OAuth2 client credentials, ensure token fetch
4. Tool input validation fails (missing required field)
5. Tool output validation fails (unexpected format)
6. Tool executes successfully and returns parsed response

---

### Deliverables

* API endpoints:

  * `POST /tools`
  * `GET /tools/{id}`
  * `PUT /tools/{id}`
  * `DELETE /tools/{id}`
  * `POST /tools/{id}/execute`
* Pydantic schema and DB models
* Execution engine with auth injectors
* Secret encryption logic
* Minimal UI or CLI for registration and testing (optional)
 


 To complete the above user story in a structured and efficient way, here's a **step-by-step development plan**, broken into phases and tasks. This plan ensures testability, modularity, and future extensibility (e.g., plugging into an LLM orchestrator later).

---

## ✅ **Phase 0: Setup & Planning**

### Goals

* Establish code structure, libraries, and storage mechanisms.
* Finalize high-level architecture.

### Tasks

* [ ] Choose FastAPI + Pydantic + SQLAlchemy (or your preferred ORM).
* [ ] Set up database (PostgreSQL or SQLite for PoC).
* [ ] Initialize encryption logic (e.g., Fernet key management).
* [ ] Create project folder structure:

```
project/
├── app/
│   ├── models/
│   ├── schemas/
│   ├── services/
│   ├── auth/
│   ├── api/
│   └── core/
├── tests/
├── main.py
└── requirements.txt
```

---

## 🧱 **Phase 1: Tool Registration Module**

### Goal

Enable creating, viewing, updating, and deleting tools.

### Tasks

* [ ] **Database Models** (`Tool`, `AuthConfig`)

  * Use discriminator for polymorphic `AuthConfig`
  * Encrypt sensitive fields
* [ ] **Pydantic Schemas**

  * ToolCreate, ToolUpdate, ToolRead
  * AuthConfig variants (bearer, api\_key, oauth2)
* [ ] **CRUD Endpoints**

  * `POST /tools`
  * `GET /tools/{id}`
  * `PUT /tools/{id}`
  * `DELETE /tools/{id}`
* [ ] **Unit Tests** for CRUD operations

---

## 🔐 **Phase 2: Authentication Module**

### Goal

Handle injecting correct authentication headers dynamically.

### Tasks

* [ ] Design abstract `AuthHandler` base class
* [ ] Implement:

  * [ ] `NoneAuthHandler`
  * [ ] `BearerTokenAuthHandler`
  * [ ] `APIKeyAuthHandler`
  * [ ] `OAuth2AuthHandler` (with token caching)
* [ ] Use dependency injection to plug correct handler at runtime
* [ ] Unit test each handler with mock data

---

## 📥 **Phase 3: Schema Validation Layer**

### Goal

Validate input before execution and output after execution.

### Tasks

* [ ] Use `jsonschema` to validate input against `input_schema`
* [ ] Use `jsonschema` to validate response against `output_schema`
* [ ] Define custom exceptions for schema validation errors
* [ ] Return 422 (input error), 500 (output error)
* [ ] Unit test with valid and invalid schemas

---

## ⚙️ **Phase 4: Tool Execution Engine**

### Goal

Enable dynamic execution of tools with runtime input.

### Tasks

* [ ] Endpoint: `POST /tools/{id}/execute`
* [ ] Steps:

  * Fetch tool
  * Validate input
  * Inject input into body/query/header
  * Attach auth using auth handler
  * Execute HTTP call using `httpx.AsyncClient`
  * Validate and return response
* [ ] Support GET, POST, PUT methods
* [ ] Handle timeouts, network errors, schema violations
* [ ] Add execution logs (basic, for debugging)

---

## 🔐 **Phase 5: Secret Management**

### Goal

Securely store and manage sensitive auth fields.

### Tasks

* [ ] Use `cryptography.fernet` to encrypt sensitive values
* [ ] Store encryption key securely (for PoC: env var is fine)
* [ ] Hide secrets in all response bodies
* [ ] Unit test encryption/decryption logic

---

## 🧪 **Phase 6: Integration Tests & CLI Tool**

### Goal

End-to-end test scenarios and ease of testing.

### Tasks

* [ ] Write integration tests for:

  * Webhook tool (no auth)
  * GET tool with API key
  * OAuth2 flow and caching
  * Input validation failure
  * Output validation failure
* [ ] Optionally: CLI or Swagger UI walkthrough

---

## 📦 **Phase 7: Wrap Up**

### Deliverables

* [ ] API Docs (Swagger/OpenAPI auto-generated)
* [ ] Developer README with setup + usage
* [ ] PoC demo instructions
* [ ] Optional: Dockerfile for containerized deployment

---

## 📅 Suggested Timeline (Fast-paced, 1–2 weeks)

| Day(s) | Phase                            |
| ------ | -------------------------------- |
| 1      | Phase 0 – Setup                  |
| 2–3    | Phase 1 – CRUD Tool Registration |
| 4–5    | Phase 2 – Authentication         |
| 6      | Phase 3 – Schema Validation      |
| 7–8    | Phase 4 – Execution Engine       |
| 9      | Phase 5 – Secrets                |
| 10     | Phase 6 – E2E Testing            |
| 11     | Phase 7 – Wrap Up & Docs         |

---

Would you like me to start scaffolding code for **Phase 1 (Tool Registration)**, or do you want to start with auth or execution first?
