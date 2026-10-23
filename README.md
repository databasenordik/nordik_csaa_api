# Nordik CSAA API

Go backend for a website and its content management tools.

The project brings pages, articles, events, and media into one API. It supports the public website as well as the tools used to maintain its content.

## What the project does

Website content is split into separate modules. This makes it easier to work on one area, such as events or galleries, without putting every feature into the same handler.

The API also includes document and book features, including PDF generation. PostgreSQL stores application data, while Google Cloud Storage is used for media files.

## Main features

| Area | Features |
| --- | --- |
| Pages and navigation | Website pages, page sections, and menus. |
| Articles and updates | Blogs, events, newsletters, and press entries. |
| Media | Galleries, images, videos, and recordings. |
| Resources | Documents and published resource entries. |
| Books | Bookshelf content, book versions, and PDF support. |
| Content listings | Lists and detail views for website content, including recent updates. |

## Technology

| Tool | Used for |
| --- | --- |
| Go 1.25 | Backend application. |
| Gin | HTTP routing and request handling. |
| GORM | Database access. |
| PostgreSQL | Application data. |
| Google Cloud Storage | Media storage. |
| Docker | Building and running the application in a container. |
| GitHub Actions | Automated checks and deployment workflows. |

## Code structure

Controllers handle HTTP requests and responses. Services contain the feature logic, while interfaces make it possible to test modules with substitute dependencies.

Shared helpers cover common work such as media handling, email, and PDF generation. Feature tests sit alongside the code they check.

| Location | Purpose |
| --- | --- |
| `cmd/server/` | Starts the HTTP server and connects the modules. |
| `internal/` | Feature modules, shared helpers, and configuration code. |
| `db-init/` | Local database setup files. |
| `Makefile` | Development, build, and test commands. |
| `.github/workflows/` | Automated checks and deployment jobs. |

## Local development

You need Go 1.25 and a configured development database. Some features also need supporting services.

```bash
git clone https://github.com/databasenordik/nordik_csaa_api.git
cd nordik_csaa_api
go mod download
```

Prepare your local development configuration before starting the server:

```bash
go run ./cmd/server
```

The default local HTTP port is `8080`.

The repository also has a Dockerfile and Compose setup. Once the development configuration is ready, start the services with:

```bash
docker compose up --build
```

Use a separate development database and sample content when working locally.

## Selected public endpoints

These read endpoints serve website content.

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/health` | Basic HTTP server health check. |
| GET | `/api/pages` | List pages. |
| GET | `/api/blogs` | List blog entries. |
| GET | `/api/events` | List events. |
| GET | `/api/galleries` | List galleries. |
| GET | `/api/resources` | List published resources. |
| GET | `/api/latest-content` | Get recent website content. |
| GET | `/api/books/public` | List public books. |

For example, check a local server:

```bash
curl http://localhost:8080/health
```

The health response is:

```json
{"status":"ok"}
```

Request a content listing:

```bash
curl http://localhost:8080/api/events
```

The returned content depends on the records in your development database.

## Tests and build

Run the commands defined in the Makefile:

```bash
make test-unit
make test-integration
```

Tests cover controllers, services, shared helpers, and document generation. Test dependencies vary by module.

Build the application:

```bash
make build
```

Check the Go code:

```bash
go vet ./...
```
