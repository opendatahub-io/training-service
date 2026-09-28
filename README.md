# OpenDataHub Training Service

OpenDataHub Training Service exposes a minimal API for submitting Ray-backed
training jobs in an OpenShift AI environment.

The service is Python-based and uses FastAPI. The public contract is maintained
in api/openapi.yaml; the server implementation is generated with OpenAPI
Generator's python-fastapi generator and kept separate from handwritten
application and backend adapters.

## Current repository state

The RHOAI 3.6 GA Developer Preview is intended for Verizon evaluation. It
provides one repository with the API and installable Helm chart. It is not an
operator and does not provide job lifecycle operations.

The planned backend boundary is:

- CodeFlare and the Ray SDKs for submitting training jobs.
- Kubernetes APIs directly for algorithm discovery and Kueue queue discovery.
- OpenShift authentication and RBAC for caller identity, project isolation, and
  permission enforcement.
- A stateless service with no service-side job database.

The estimate endpoint is a stretch goal. See docs/architecture.md for the
scope and ownership rules, and docs/installation.md for Helm installation.

## Local development

Requirements: Python 3.11+ and uv.

    make install
    make check
    make run

The local service listens on http://localhost:8080. The health endpoint is
available at http://localhost:8080/healthz.
