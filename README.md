# FluentXVerse Go Server

A Go Fiber backend migration workstream for FluentXVerse. This repository makes the migration code independently inspectable; it does not claim that the Go service has replaced the active Bun/Elysia application API.

## Scope

- Fiber bootstrap, CORS, request IDs, security headers, panic recovery, and structured logging.
- Memgraph, PostgreSQL, and Redis clients.
- Authentication helpers and route groups intended to mirror application contracts.
- Scheduling, tutor/student operations, content, notifications, and native WebSocket signaling.
- AI assessment/transcription adapters and proof bridge experiments.
- GMR Engine integration for externally managed Web3 operations.

Implementation presence does not establish full behavioral parity. Compare routes, authentication, booking/refund atomicity, classroom protocols, data ownership, and security controls against the active [Bun server](https://github.com/fluentxverse/fluentxverse-server) before promoting any route group.

## Local Development

Use Go 1.25.1+ and an isolated development environment. From the repository root:

```bash
cd fluentxverse-go-server
cp .env.example .env
go mod download
go test ./...
go build ./cmd/api
go run ./cmd/api
```

The default HTTP port is 8765; choose a different port if the Bun server is already using it. Configure a dedicated development database and secrets in `.env`. Do not run both backends against mutable production data to test parity.

Node.js dependencies in this directory support legacy proof bridge experiments. Generated Circom build files and development proving/setup artifacts are not exported. Do not enable proof routes without explicitly provisioned, reviewed artifacts. Existing development setup generation is not suitable for a production proof system.

## Migration and Security Boundaries

The retained nested guide describes the original migration intention, not the current production routing. Historical fallback paths to sibling Bun-server configuration/assets refer to the monorepo; configure this service explicitly when running it independently. The standalone repository does not contain a production deployment pipeline.

New custody, signing, verification-network credentials, and chain writes belong in GMR Engine/GMR Vault. Legacy proof bridge code that can read a verification-network seed phrase is migration debt, not the target architecture. Do not configure mainnet credentials in this service to bypass that boundary. Keep primary marketplace and compensation records in Memgraph, with PostgreSQL primarily supporting logs and operational records.

## Repository Provenance

This repository is an app-focused export of [the FluentXVerse monorepo](https://github.com/fluentxverse/fluentxverse-monorepo). The monorepo remains the source of truth; submit cross-application changes there. Existing directory names are retained so shared imports remain valid. The historical student/tutor folder suffix does not indicate a Preact app: both active web clients use SolidJS.

Exported source commit: [`c9696e491149`](https://github.com/fluentxverse/fluentxverse-monorepo/tree/c9696e491149275bc01df3b8d05d29a15263257a). See [SOURCE.json](SOURCE.json) for the exact scope. Local uncommitted edits, environment files, runtime data, curriculum backups, and generated proving artifacts are not published. Original Git history is not copied.

Cross-app parity tests and full deployment orchestration remain in the monorepo. The checks in this repository do not establish production acceptance, security audit completion, or proof-system review. No production deployment is triggered by this export.

## Security and Licensing

Never commit API tokens, seed phrases, private keys, student records, recordings, or real assessment evidence. Use synthetic fixtures in tests. Report security issues privately to hello@fluentxverse.com rather than opening an issue containing sensitive data.

No root license file was present in the source revision. Public source availability, if enabled, is not itself a license grant. Contact FluentXVerse for licensing terms; third-party dependencies retain their own licenses.
