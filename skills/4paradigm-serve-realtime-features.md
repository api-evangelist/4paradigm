---
name: serve-realtime-features-openmldb
description: Insert rows, call a deployed real-time feature service, and run online or offline SQL against an OpenMLDB cluster over its REST APIServer.
api: 4paradigm-openmldb-apiserver
generated: '2026-09-05'
method: generated
source: grounded in 4Paradigm's own REST reference at https://openmldb.ai/docs/en/main/quickstart/sdk/rest_api.html (fetched 2026-09-05). OpenMLDB publishes no OpenAPI, so operations are named by the documented HTTP method and path — none is invented.
operations:
  - 'PUT /dbs/{db_name}/tables/{table_name}'
  - 'POST /dbs/{db_name}/deployments/{deployment_name}'
  - 'GET /dbs/{db_name}/deployments/{deployment_name}'
  - 'POST /dbs/{db_name}'
  - 'GET /dbs'
  - 'GET /dbs/{db}/tables'
  - 'POST /refresh'
---

# Serve real-time features from OpenMLDB over REST

## Read this first

4Paradigm's own documentation says APIServer "is mainly used for functional testing, not recommended
for performance testing, nor recommended for the production environment", and that the default
deployment has no high-availability mechanism. For production traffic use the Java, Python, Go or C++
SDK against the cluster directly. Do not put an agent's production path on this surface without saying
so out loud.

## Steps

1. **Find the schema.** `GET /dbs` lists databases; `GET /dbs/{db}/tables` returns each table with its
   `column_desc`, `column_key`, index `ttl` and partition/replica counts.
2. **Insert a row.** `PUT /dbs/{db_name}/tables/{table_name}` with
   `{"value": [[v1, v2, v3]]}`. One row per call, and the values must be in strict schema order —
   there is no named-column form of insert.
3. **Read the contract of a deployed service.** `GET /dbs/{db_name}/deployments/{deployment_name}`
   returns `input_schema`, `output_schema` and the common-column lists. Call this before you compute;
   it is the only machine-readable description of that deployment's inputs.
4. **Compute.** `POST /dbs/{db_name}/deployments/{deployment_name}` with either the array form
   (`{"input": [[...], [...]]}`) or the JSON form (`{"input": [{"col0": ..., ...}]}`). Never mix the two
   in one request — the response comes back in whichever form you sent. Set `need_schema: true` if you
   want the output schema alongside the rows.
5. **Ad-hoc SQL.** `POST /dbs/{db_name}` with `{"mode": "online" | "offsync" | "offasync", "sql": ...}`,
   plus `input.schema` and `input.data` for a parameterised query.
6. **After a DDL change.** `POST /refresh`. Creating or dropping a table or a deployment is not
   immediately visible to APIServer; if it cannot find something you just created, refresh before
   concluding it failed.

## Rules this API imposes

- **The response envelope is `{"code": 0, "msg": "ok", "data": {...}}`** — check `code`, not just the
  HTTP status. It is not RFC 9457.
- **The JSON is not strict JSON.** Responses may contain unquoted `NaN`, `Infinity` and `-Infinity`.
  Send `"write_nan_and_inf_null": true` to get `null` instead, or your parser will fail.
- **Timestamps must be integers** (`1635247427000`); dates must be a `YYYY-MM-DD` string with no
  spaces. `true`/`false`/`null` are lowercase-only.
- **Floating-point precision loss is explicitly not rejected** — `0.3` comes back as
  `0.30000000000000004`, and a value above float max becomes `Inf`.
- **No idempotency and no request id.** A retried insert is a second insert. Deletion is by index key
  via SQL `DELETE`, so plan the undo before you write.
