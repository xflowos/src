# DO - Schema-Driven DevOps Orchestrator

**One schema. One command. Everything generated.**

[![Go Version](https://img.shields.io/github/go-mod/go-version/xflowos/do)](https://go.dev/)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Go Report Card](https://goreportcard.com/badge/github.com/xflowos/do)](https://goreportcard.com/report/github.com/xflowos/do)
[![CI Status](https://github.com/xflowos/do/workflows/CI/badge.svg)](https://github.com/xflowos/do/actions)

---

## What is DO?

DO is a **schema-driven code generator** that transforms a single declarative schema file into production-ready Go applications. Think Infrastructure-as-Code, but for your entire application stack.

**Write this:**

```yaml
# app.schema
app:
  name: greet
  version: 1.0.0
  description: Simple greeting API

endpoints:
  - name: hello
    method: GET
    path: /hello/{name}
    params:
      - name: name
        type: string
        required: true
    response:
      message: string
      timestamp: int64
```

**Get this:**

- ✅ Complete Goa API (design, server, client)
- ✅ OpenAPI 3.0 specification
- ✅ Type-safe CLI (Fang-powered)
- ✅ Interactive TUI (Bubble Tea)
- ✅ HTTP handlers with business logic scaffolds
- ✅ Comprehensive tests
- ✅ Docker & Kubernetes manifests
- ✅ CI/CD pipelines (GitHub Actions)
- ✅ Complete documentation

**All from one schema. Zero boilerplate. 100% type-safe.**

---

## Quick Start

### Installation

```bash
# Install via Go
go install github.com/xflowos/do/cmd/do@latest

# Or download binary from releases
curl -sSL https://github.com/xflowos/do/releases/latest/download/do_linux_amd64 -o do
chmod +x do
sudo mv do /usr/local/bin/
```

### Create Your First App

```bash
# Interactive mode (recommended)
do init --interactive

# Or non-interactive
do init myapp \
  --desc "My awesome API" \
  --author "Your Name" \
  --license MIT
```

This creates `app.schema` and launches an **interactive Bubble Tea UI** to define your endpoints.

### Generate Everything

```bash
do generate app.schema

# Output:
# ✓ Generated design/design.go (Goa)
# ✓ Generated gen/ (Goa artifacts)
# ✓ Generated cmd/ui/ (Bubble Tea UI)
# ✓ Generated cmd/cli/ (Fang CLI)
# ✓ Generated handlers/ (business logic scaffolds)
# ✓ Generated openapi.yaml
# ✓ Generated tests/
# ✓ Generated Dockerfile
# ✓ Generated .github/workflows/
```

### Implement Business Logic

**Only file you write:**

```go
// handlers/greet.go
package handlers

import (
    "context"
    "time"
    "myapp/gen/greet"
)

type GreetService struct{}

func (s *GreetService) Hello(ctx context.Context, p *greet.HelloPayload) (*greet.HelloResult, error) {
    return &greet.HelloResult{
        Message:   "Hello, " + p.Name + "!",
        Timestamp: time.Now().Unix(),
    }, nil
}
```

### Run Your App

```bash
# Development server with live reload
do serve

# Or run generated binary
go run cmd/server/main.go

# Or use the generated CLI
go run cmd/cli/main.go hello "World"

# Or use the interactive TUI
go run cmd/ui/main.go
```

---

## Key Features

### 🎯 One Schema, Multiple Outputs

```
app.schema
    ├─→ Goa API (design.go)
    ├─→ OpenAPI 3.0 (openapi.yaml)
    ├─→ Fang CLI (type-safe commands)
    ├─→ Bubble Tea UI (interactive forms)
    ├─→ HTTP Server (handlers + middleware)
    ├─→ Tests (unit + integration)
    ├─→ Docker (Dockerfile + compose)
    ├─→ Kubernetes (manifests)
    └─→ CI/CD (.github/workflows)
```

### 🚀 Zero Boilerplate

- **Before DO:** 2000+ lines of repetitive code
- **With DO:** 23 lines of schema + business logic
- **Code amplification:** ~100x

### 🔒 Type-Safe Everything

- Schema validated at generation time
- Go's type system enforces correctness
- No runtime type errors
- Full IDE autocomplete support

### 🎨 Beautiful Developer Experience

- **Interactive schema builder** (Bubble Tea TUI)
- **Live reload** during development
- **Comprehensive error messages**
- **Auto-generated documentation**

### 🏗️ Production-Ready Output

- **Structured logging** (built-in)
- **Metrics & observability** (Prometheus)
- **Health checks** (liveness + readiness)
- **Graceful shutdown**
- **Rate limiting**
- **Authentication middleware** (JWT, OAuth2)

---

## Architecture

```
DO v0.5.0
│
├─ Internal Packages
│  ├─ schema/       Parse & validate app.schema
│  ├─ openapi/      Generate OpenAPI 3.0 spec
│  ├─ generator/    Template engine + code gen
│  ├─ ui/           Bubble Tea interactive builder
│  └─ db/           Template cache & storage
│
├─ Commands (Fang)
│  ├─ init          Create new schema
│  ├─ generate      Generate all code
│  ├─ validate      Validate schema
│  ├─ serve         Dev server with live reload
│  └─ schema        Schema management
│
└─ Templates (Embedded)
   ├─ goa/          Goa design.go
   ├─ bubbletea/    Interactive UI
   ├─ fang/         CLI commands
   ├─ handlers/     HTTP handlers
   ├─ docker/       Containerization
   ├─ k8s/          Kubernetes
   └─ ci/           GitHub Actions
```

---

## CLI Reference

### `do init`

Create new application schema.

```bash
# Interactive mode (recommended)
do init --interactive

# Non-interactive
do init myapp \
  --desc "My API" \
  --author "Me" \
  --license MIT \
  --output myapp.schema
```

**Options:**
- `-i, --interactive` - Launch Bubble Tea UI
- `-d, --desc` - Project description
- `-a, --author` - Author name
- `-l, --license` - License (MIT, Apache-2.0, GPL-3.0)
- `-o, --output` - Output filename (default: app.schema)

### `do generate`

Generate code from schema.

```bash
# Generate everything
do generate app.schema

# Generate specific components
do generate app.schema --goa --ui --cli
do generate app.schema --handlers --tests
```

**Options:**
- `-a, --all` - Generate everything (default)
- `-g, --goa` - Generate Goa design.go
- `-u, --ui` - Generate Bubble Tea UI
- `-c, --cli` - Generate Fang CLI
- `-h, --handlers` - Generate handler scaffolds
- `-t, --tests` - Generate tests
- `-d, --docker` - Generate Docker files
- `-k, --k8s` - Generate Kubernetes manifests
- `-o, --output` - Output directory (default: .)
- `-f, --force` - Overwrite existing files

### `do validate`

Validate schema file.

```bash
do validate app.schema

# Output:
# ✓ Schema valid: myapp v1.0.0
#   Endpoints: 3
#   Types: 2
#   Database: postgres
```

### `do serve`

Start development server with live reload.

```bash
do serve

# With custom config
do serve --config dev.yaml --port 8080
```

**Options:**
- `-p, --port` - Server port (default: 8080)
- `-c, --config` - Config file
- `--reload` - Enable live reload (default: true)

### `do schema`

Schema management commands.

```bash
# Edit schema interactively
do schema edit app.schema

# Show schema details
do schema show app.schema
do schema show app.schema --format json
do schema show app.schema --format table

# Validate
do schema validate app.schema
```

---

## Schema Reference

### Complete Example

```yaml
app:
  name: myapp
  version: 1.0.0
  description: My awesome API
  author: Your Name
  license: MIT

# Database configuration
database:
  type: postgres
  migrations: ./migrations

# Authentication
auth:
  type: jwt
  secret: ${JWT_SECRET}

# API endpoints
endpoints:
  - name: create_user
    method: POST
    path: /users
    description: Create a new user
    auth: true  # Requires authentication
    
    body:
      type: object
      properties:
        name:
          type: string
          validation:
            min: 3
            max: 50
        email:
          type: string
          validation:
            pattern: ^[^\s@]+@[^\s@]+\.[^\s@]+$
        age:
          type: int
          validation:
            min: 18
            max: 120
    
    response:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        email:
          type: string
        created_at:
          type: time

  - name: get_user
    method: GET
    path: /users/{id}
    auth: true
    
    params:
      - name: id
        type: string
        required: true
    
    response:
      $ref: '#/types/User'

# Custom types (reusable)
types:
  - name: User
    properties:
      id:
        type: string
      name:
        type: string
      email:
        type: string
      created_at:
        type: time

# Deployment configuration
deployment:
  platform: kubernetes
  replicas: 3
  resources:
    memory: 512Mi
    cpu: 500m
```

### Supported Types

- `string` - UTF-8 strings
- `int` - 32-bit integers
- `int64` - 64-bit integers
- `float64` - Double precision floats
- `bool` - Booleans
- `time` - RFC3339 timestamps
- `uuid` - UUID v4
- `object` - Nested objects
- `array` - Lists

### Validation Rules

```yaml
validation:
  min: 1              # Minimum value/length
  max: 100            # Maximum value/length
  pattern: ^[a-z]+$   # Regex pattern
  enum: [a, b, c]     # Allowed values
```

---

## Generated Project Structure

```
myapp/
├── app.schema              # Your schema (source of truth)
│
├── design/
│   └── design.go          # Generated Goa design
│
├── gen/                   # Generated by Goa
│   ├── myapp/
│   │   ├── client.go
│   │   ├── endpoints.go
│   │   └── service.go
│   └── http/
│       ├── openapi.yaml   # OpenAPI 3.0 spec
│       └── ...
│
├── cmd/
│   ├── server/
│   │   └── main.go        # HTTP server
│   ├── cli/
│   │   └── main.go        # Fang CLI
│   └── ui/
│       └── main.go        # Bubble Tea UI
│
├── handlers/              # Business logic (you write this)
│   └── myapp.go
│
├── internal/
│   ├── middleware/        # Generated middleware
│   ├── config/            # Configuration
│   └── db/                # Database
│
├── tests/                 # Generated tests
│   ├── unit/
│   └── integration/
│
├── deployments/
│   ├── docker/
│   │   └── Dockerfile
│   └── k8s/
│       ├── deployment.yaml
│       └── service.yaml
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── release.yml
│
├── go.mod
├── go.sum
├── Makefile               # Generated build commands
└── README.md              # Generated documentation
```

---

## Development Workflow

### 1. Define Your API

```bash
do init --interactive
```

Use the Bubble Tea UI to define endpoints, types, and configurations.

### 2. Generate Code

```bash
do generate app.schema
```

This creates all boilerplate, infrastructure code, and scaffolds.

### 3. Implement Business Logic

Only write code in `handlers/`:

```go
func (s *Service) CreateUser(ctx context.Context, p *Payload) (*Result, error) {
    // Your business logic here
}
```

### 4. Test

```bash
# Run generated tests
go test ./...

# Run specific test
go test ./tests/unit/...

# With coverage
go test -cover ./...
```

### 5. Develop with Live Reload

```bash
do serve --reload
```

Changes to `handlers/` trigger automatic rebuilds.

### 6. Deploy

```bash
# Build
make build

# Docker
make docker-build
docker-compose up

# Kubernetes
kubectl apply -f deployments/k8s/
```

---

## Testing

DO generates comprehensive tests:

### Unit Tests

```go
// tests/unit/handlers_test.go (GENERATED)
func TestCreateUser(t *testing.T) {
    svc := handlers.NewService()
    
    result, err := svc.CreateUser(context.Background(), &CreateUserPayload{
        Name:  "Alice",
        Email: "alice@example.com",
    })
    
    assert.NoError(t, err)
    assert.NotEmpty(t, result.ID)
}
```

### Integration Tests

```go
// tests/integration/api_test.go (GENERATED)
func TestCreateUserAPI(t *testing.T) {
    client := NewTestClient(t)
    
    resp, err := client.CreateUser(&CreateUserRequest{
        Name:  "Alice",
        Email: "alice@example.com",
    })
    
    assert.NoError(t, err)
    assert.Equal(t, 201, resp.StatusCode)
}
```

### Run Tests

```bash
# All tests
make test

# Unit tests only
make test-unit

# Integration tests
make test-integration

# With coverage
make test-coverage

# Watch mode
make test-watch
```

---

## CI/CD

DO generates complete GitHub Actions workflows:

### `.github/workflows/ci.yml`

```yaml
# GENERATED by DO
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.23'
      
      - name: Run tests
        run: make test
      
      - name: Upload coverage
        uses: codecov/codecov-action@v4
  
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: golangci/golangci-lint-action@v4
  
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build
        run: make build
```

### `.github/workflows/release.yml`

```yaml
# GENERATED by DO
name: Release

on:
  push:
    tags: ['v*']

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: goreleaser/goreleaser-action@v5
        with:
          args: release --clean
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### GoReleaser Config

DO generates `.goreleaser.yaml`:

```yaml
# GENERATED by DO
project_name: myapp

builds:
  - main: ./cmd/server
    binary: myapp-server
    goos: [linux, darwin, windows]
    goarch: [amd64, arm64]
  
  - main: ./cmd/cli
    binary: myapp
    goos: [linux, darwin, windows]
    goarch: [amd64, arm64]

archives:
  - format: tar.gz
    name_template: '{{ .ProjectName }}_{{ .Version }}_{{ .Os }}_{{ .Arch }}'

release:
  github:
    owner: yourusername
    name: myapp
```

---

## Docker & Kubernetes

### Generated Dockerfile

```dockerfile
# GENERATED by DO
FROM golang:1.23-alpine AS builder

WORKDIR /app
COPY . .
RUN go mod download
RUN CGO_ENABLED=0 go build -o server ./cmd/server

FROM alpine:latest
RUN apk --no-cache add ca-certificates
COPY --from=builder /app/server /server
EXPOSE 8080
CMD ["/server"]
```

### Generated Kubernetes Manifests

```yaml
# deployments/k8s/deployment.yaml (GENERATED)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
```

---

## Tech Stack

DO leverages the best Go libraries:

| Component | Library | Purpose |
|-----------|---------|---------|
| **CLI Framework** | [Fang](https://github.com/charmbracelet/fang) | Type-safe commands |
| **TUI** | [Bubble Tea](https://github.com/charmbracelet/bubbletea) | Interactive UI |
| **Forms** | [Huh](https://github.com/charmbracelet/huh) | Input forms |
| **Styling** | [Lipgloss](https://github.com/charmbracelet/lipgloss) | Terminal styling |
| **API Framework** | [Goa](https://goa.design) | Design-first APIs |
| **HTTP Router** | Generated by Goa | Type-safe routing |
| **Schema** | [yaml.v3](https://gopkg.in/yaml.v3) | YAML parsing |
| **Testing** | [testify](https://github.com/stretchr/testify) | Assertions |
| **Build** | [GoReleaser](https://goreleaser.com) | Release automation |

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md).

### Development Setup

```bash
# Clone
git clone https://github.com/xflowos/do.git
cd do

# Install dependencies
go mod download

# Run tests
make test

# Build
make build

# Install locally
make install
```

### Running Tests

```bash
# All tests
make test

# With coverage
make test-coverage

# Integration tests
make test-integration

# Benchmarks
make bench
```

### Code Quality

```bash
# Lint
make lint

# Format
make fmt

# Vet
make vet

# All checks
make check
```

---

## License

GPL-3.0 - See [LICENSE](LICENSE)

---

## Support

- **Documentation:** [docs.xflow.dev](https://docs.xflow.dev)
- **Issues:** [GitHub Issues](https://github.com/xflowos/do/issues)
- **Discussions:** [GitHub Discussions](https://github.com/xflowos/do/discussions)
- **Email:** support@xflow.dev

---

## Roadmap

### v0.5.0 (Current)
- [x] Schema parser & validator
- [x] Goa design.go generation
- [x] Bubble Tea interactive builder
- [x] Fang CLI generation
- [x] OpenAPI 3.0 generation
- [x] Handler scaffolds
- [x] Test generation
- [x] Docker & K8s manifests
- [x] CI/CD workflows

### v0.6.0
- [ ] +git features.
- [ ] flow intergration.
  - [ ] zit
  - [ ] zfs support
  - [ ] 9p afero intergration
- [ ] Authentication middleware (OAuth2, OIDC)
- [ ] Caching layer

### v0.7.0 
- [ ] Schema versioning & migrations
- [ ] Full bubbletea ... component intergration
- [ ] agentic tooling

### v0.8.0
- [ ] flowlang
  - [ ] langpacks

### v0.9.0
- [ ] flow intergration
  - [ ] pkg
  - [ ] cfg
  - [ ] run
  - [ ] send
  - [ ] recv

### v1.0.0 (Q4 2026)
- [ ] Production hardening
- [ ] Performance optimization
- [ ] storyboard

---

**Made with ❤️ for Gifted Superheros**
