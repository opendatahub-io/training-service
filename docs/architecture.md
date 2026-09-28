# RHOAI 3.6 GA Developer Preview

The Developer Preview is the minimum implementation needed for Verizon to
evaluate the Training API. It is one installable repository containing the
Python service and its Helm chart. It is not an operator.

The service is intentionally split into generated HTTP plumbing and handwritten
application behavior:

    training-service/
    ├── api/
    │   ├── openapi.yaml                    # public contract
    │   └── openapi-generator-config.yaml  # python-fastapi generation settings
    ├── charts/training-service/            # installable Helm chart
    ├── docs/
    │   ├── architecture.md
    │   └── installation.md
    ├── src/training_service/
    │   ├── main.py                         # application composition and probes
    │   ├── api_impl/                       # implementations called by generated routes
    │   ├── application/                    # training submission use case
    │   ├── domain/                         # submission models and validation
    │   └── adapters/
    │       ├── kubernetes/                 # K8s discovery, identity, and RBAC
    │       └── ray/                        # CodeFlare and Ray SDK submission
    ├── tests/unit/                         # focused unit tests only
    ├── Containerfile
    ├── Makefile
    └── pyproject.toml

## API scope and endpoint ownership

- POST /projects/{project}/training-jobs is the core Developer Preview path.
  It submits to Ray through the CodeFlare and Ray SDKs.
- GET /algorithms and GET /projects/{project}/queues are read-only discovery
  helpers implemented with Kubernetes APIs directly.
- POST /training-jobs/estimate is a stretch goal and does not submit a job.

The service must not construct or manage RayJob custom resources, Ray job
manifests, or any other job custom resource. Ray cluster lifecycle is outside
this repository.

## Security model

- Require an OpenShift bearer token for API calls.
- Derive caller identity from the authenticated token; never trust a caller
  identity supplied as an ordinary request field.
- Propagate the caller identity/token to the Kubernetes, CodeFlare, and Ray
  submission path as required by the platform integration.
- Enforce the project path against the caller's existing OpenShift RBAC
  permissions.
- Keep the service stateless and do not grant users permissions beyond their
  existing OpenShift access.

The generated package must not contain Kubernetes calls or business rules.
Those belong in the application and adapter layers so the OpenAPI contract can
change without coupling the service to a particular execution backend.

## Explicit non-goals for the Developer Preview

- No job list, get-status, delete, cancel, pause, resume, or log endpoints.
- No job lifecycle management.
- No operator.
- No Ray custom resource or manifest construction/management.
- No Kubeflow/automatic backend selection.
- No persistent service-side job state.
- No Helm test suite.
- No end-to-end cluster test matrix.
