---
name: provision-openaios-dev-environment
description: Provision, inspect and tear down a development environment on a self-hosted OpenAIOS-Platform deployment.
api: 4paradigm-openaios-platform
generated: '2026-09-05'
method: generated
source: grounded in operationIds present in openapi/4paradigm-openaios-platform.yaml (verified 2026-09-05)
operations:
  - GetComputingUnitSpecs
  - createEnvironment
  - getEnvironment
  - getEnvironmentList
  - getTerminal
  - deleteEnvironment
---

# Provision an OpenAIOS development environment

An environment is a named, running workspace (an image plus a compute unit plus mounted storage) on an
OpenAIOS-Platform cluster. Everything is addressed by NAME, not by an opaque id.

## Before you start

- **There is no 4Paradigm-hosted endpoint.** The base URL is your own deployment; the spec ships
  `http://127.0.0.1:1234/api` because the platform is self-hosted.
- Send `Authorization: <key>` (scheme `ApiKeyAuth`) or an OIDC bearer token from your cluster's own
  identity provider. 4Paradigm issues neither.

## Steps

1. **See what you can ask for.** `GetComputingUnitSpecs` (`GET /computing_resource/specs`) returns the
   compute-unit specs the cluster offers. Pick the `ComputeUnitId` you will pass in the environment
   config — do not guess one.
2. **Create it.** `createEnvironment` (`POST /environments/{name}`). The body is an
   `EnvironmentConfig`: an `image` (`ImageConfig`), a `compute_unit` (`ComputeUnitId`) and optional
   `mounts` (`StorageMapping[]`). `{name}` is the identity of the environment — choose it deliberately.
3. **Wait for it.** `getEnvironment` (`GET /environments/{name}`) returns an `EnvironmentRuntimeInfo`
   carrying `state` (`EnvironmentState`), `sshInfo` and an `events` list. Poll this rather than assuming
   the create succeeded; there is no callback, webhook or async job id.
4. **Get in.** `getTerminal` (`GET /web-terminal/terminal?pod=<podName>`, a separate service on the same
   host) returns a browser terminal URL for a pod.
5. **List what exists.** `getEnvironmentList` (`GET /environments`).
6. **Tear it down.** `deleteEnvironment` (`DELETE /environments/{name}`).

## Rules this API imposes

- **No idempotency key.** A retried `POST /environments/{name}` is a second real create attempt. Because
  the name IS the key, a repeat will usually conflict (409) rather than duplicate — that conflict is
  your only replay protection, and it is incidental rather than promised.
- **Delete is final.** `deleteEnvironment` has no soft-delete, no restore and no stated window. Confirm
  with `getEnvironment` before you call it.
- **Errors are not uniform.** 400 returns JSON `{type, message, content}`; 401 and 500 return
  `text/plain` with no error id. See `errors/4paradigm-problem-types.yml`.
- **Two pagination styles exist in this API** (`offset`/`limit` and `page`/`page_size`). Read the
  parameter list of the specific operation; do not carry one style across.
