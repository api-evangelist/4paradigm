---
name: deploy-openaios-app-from-store
description: Find a Helm chart in the OpenAIOS app store, release it, and read back the running application's pods, services and notes.
api: 4paradigm-openaios-platform
generated: '2026-09-05'
method: generated
source: grounded in operationIds present in openapi/4paradigm-openaios-platform.yaml (verified 2026-09-05)
operations:
  - getAppstoreChartList
  - getAppstoreChart
  - createRelease
  - getRelease
  - getApplicationList
  - getApplicationPods
  - getApplicationServices
  - getApplicationNotes
  - GetContainerLog
  - deleteRelease
---

# Deploy an application from the OpenAIOS app store

The app store is a Helm chart repository; an application is a Helm release of one of its charts.

## Steps

1. **Browse.** `getAppstoreChartList` (`GET /appstore/charts`) — paginated with `offset` and `limit`.
   Returns `ChartMetaDataList`; each `ChartMetadata` carries a `category` (`ChartCategory`).
2. **Read one chart.** `getAppstoreChart`
   (`GET /appstore/charts/{category}/{name}/{version}`) returns a `Chart` (`metadata` + `files`). Charts
   are addressed by the triple, so pin the version rather than assuming a latest.
3. **Release it.** `createRelease` (`POST /releases/{name}`) with a `ReleaseCreateConfig`
   (`name` + `chart_name`). `{name}` becomes the application's identity.
4. **Confirm.** `getRelease` (`GET /releases/{name}`), then `getApplicationList`
   (`GET /applications`) to see it alongside the rest.
5. **Inspect the running thing.** `getApplicationPods` (`KubernetesPod[]` with `state`, `compute_unit`
   and `containers`), `getApplicationServices` (`KubernetesService[]`), `getApplicationNotes` (the
   chart's rendered NOTES.txt — this is where a chart tells you its own access URL and credentials), and
   `getApplicationMetadata`.
6. **Debug.** `GetContainerLog` (`GET /log/pod/{pod_name}`).
7. **Remove.** `deleteRelease` (`DELETE /releases/{name}`), or `deleteApplication`
   (`DELETE /applications/{name}`) for the instance.

## Rules this API imposes

- **Uploading a chart is a write to a shared catalogue.** `uploadChart`
  (`POST /appstore/charts/{category}`) publishes for everyone on the cluster. There is no draft state.
- **`deleteRelease` is the only reversal of `createRelease`**, and no window is stated. Helm itself
  supports rollback; this API exposes none, so a bad release is a delete-and-recreate, not an undo.
- **Do not treat a 500 as retryable on a write.** The body is a bare `text/plain` string with no error
  id or request id, so you cannot tell a failed create from a succeeded-then-failed-to-respond create.
  Re-read with `getRelease` before retrying.
