---
name: manage-openaios-container-images
description: Import container images into an OpenAIOS project, watch the import, cancel it, and manage the resulting image list.
api: 4paradigm-openaios-platform
generated: '2026-09-05'
method: generated
source: grounded in openapi/4paradigm-openaios-platform.yaml (verified 2026-09-05); operations without an operationId are addressed by method and path, as the published spec leaves them unnamed
operations:
  - ListImportingImages
  - 'POST /images/importing'
  - 'PUT /images/importing'
  - 'DELETE /images/importing'
  - 'GET /images'
  - 'PUT /images'
  - 'DELETE /images'
  - 'GET /images/registry'
  - 'GET /images/info'
  - 'GET /public_images'
---

# Manage container images on OpenAIOS-Platform

## Steps

1. **Find the registry.** `GET /images/registry` returns the registries the platform can pull from
   (`ImageRegistryInfo`); `GET /images/info` returns your own project, `GET /public_images/info` the
   public one.
2. **Start an import.** `POST /images/importing` with the registry, repo and tag
   (`ImageImportingInfo`).
3. **Watch it.** `ListImportingImages` (`GET /images/importing`) — paginated with `offset`/`limit`.
   Match on the `importing_id` returned for your import.
4. **Cancel it if you must.** `PUT /images/importing` stops an import in flight. This is the only
   genuine in-flight cancel in the whole OpenAIOS surface — use it rather than letting a bad import
   finish.
5. **Clear it from the list.** `DELETE /images/importing`.
6. **Work with the results.** `GET /images` and `GET /public_images` list images (paginated with
   `page`/`page_size` — a DIFFERENT style from step 3, in the same API); `PUT /images` copies an image;
   `DELETE /images` deletes one.

## Rules this API imposes

- **The pagination style changes between neighbouring operations.** `/images/importing` takes
  `offset`/`limit`; `/images` takes `page`/`page_size`. Neither returns a total or a next link, so
  page until you get a short page.
- **`DELETE /images` is unqualified and irreversible.** There is no trash, no restore and no stated
  retention. Copy first (`PUT /images`) if you might want it back.
- **No image is signed or digest-pinned by this API** — it addresses images by repo and tag.
