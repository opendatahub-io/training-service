# API contract

openapi.yaml is the source of truth for the public Training Service HTTP
contract.

For the RHOAI 3.6 GA Developer Preview the contract is intentionally limited to
training submission, read-only algorithm/queue discovery, and the optional
estimate stretch goal. It does not contain job lifecycle endpoints.

The repository uses the OpenAPI Generator python-fastapi server generator.
Generated output is deliberately kept separate from handwritten orchestration
and adapters. Regenerate it with:

    make generate-server

The generator is currently beta. Review generated changes as part of the pull
request that changes the API contract.
