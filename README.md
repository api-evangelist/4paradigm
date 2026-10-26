# 4Paradigm

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

4Paradigm (Beijing Fourth Paradigm Intelligent Technology, HKEX 6682) is a Chinese enterprise AI
company building decision-making and generative AI platforms — Sage AIOS, Sage HyperCycle ML/CV/OCR,
SageGPT and the SageOne appliances — for banking, insurance, securities, retail, energy, healthcare
and manufacturing.

- Company site: https://www.4paradigm.com/
- Open-source community: https://github.com/4paradigm
- Investor relations: http://ir.4paradigm.com/en/index.html

## What this profile covers

4Paradigm's commercial Sage products are sold through a sales motion and publish no API. Everything
machine-readable in this profile comes from its open-source projects, all of which are self-hosted:

| Surface | Contract | State |
| --- | --- | --- |
| **OpenMLDB** — ML feature database | 8 Protobuf services (140 RPCs) in `grpc/`; a documented REST APIServer with no OpenAPI | Apache-2.0; last release v0.9.3, 2025-02-21 |
| **OpenAIOS-Platform (Pineapple)** — Kubernetes AI development platform | 4 first-party OpenAPI 3.0.3 documents (57 operations) in `openapi/` | Apache-2.0; repository dormant since 2021-08-20 |
| **PhanthyMotus** — embodied-AI agent framework | 16 driver bundles, each an MCP server, declaring 303 typed cards (`mcp/`) | Apache-2.0; shipping daily |
| **Sage App Store catalogue** | none published; five endpoints probed live and unauthenticated | live at https://apps.4paradigm.com/api |

Notable findings from the 2026-09-05 enrichment pass:

- The OpenMLDB REST APIServer is deprecated for production **by 4Paradigm's own documentation**.
- No `/.well-known/` document is served on any host; `www.4paradigm.com` answers 200 with an HTML
  catch-all for every path, which is a soft-404 rather than a document.
- No idempotency, no rate-limit signalling, no status page, no deprecation policy and no published
  pricing anywhere in the estate.
- 4Paradigm publishes no A2A agent card; none was authored on its behalf.
