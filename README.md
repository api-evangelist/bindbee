# Bindbee (bindbee)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Bindbee provides a unified HRIS and ATS integration API that allows companies to connect with 50+ HR systems including BambooHR, Workday, ADP, Greenhouse, and Lever through a single normalized API, simplifying workforce data access and HR automation.

**URL:** [https://bindbee.dev/](https://bindbee.dev/)

## Tags:

 - ATS, HR Integration, HRIS, Workforce, Recruiting, People Ops

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### Bindbee API
Unified HRIS and ATS integration API providing normalized access to employee records, departments, time-off, job postings, and candidates from 50+ HR systems.

**Human URL:** [https://bindbee.dev/](https://bindbee.dev/)

#### Tags:

 - ATS, HR Integration, HRIS, Workforce

#### Properties

- [Documentation](https://docs.bindbee.dev/)
- [OpenAPI](openapi/bindbee-api.yaml)

## Common Properties

- [Portal](https://bindbee.dev/)
- [Documentation](https://docs.bindbee.dev/)

## Features

| Name | Description |
|------|-------------|
| Unified HRIS API | Access employee data from BambooHR, Workday, ADP, and 50+ HRIS systems through one API. |
| Unified ATS API | Access job listings and candidates from Greenhouse, Lever, Workable, and other ATS systems. |
| Data Normalization | Consistent normalized schema across all connected HR systems. |
| Cursor Pagination | Efficient cursor-based pagination for large employee datasets. |
| Connector Tokens | Secure per-integration connector tokens for multi-tenant HR data access. |
| Real-Time Sync | Webhooks and polling for real-time HR data synchronization. |

## Use Cases

| Name | Description |
|------|-------------|
| Employee Directory Integration | Sync employee records from any HRIS into internal apps and directories. |
| Onboarding Automation | Trigger onboarding workflows when new employees are added in the HRIS. |
| Recruiting Pipeline Visibility | Track candidates across ATS stages in unified dashboards. |
| HRIS Migration | Move between HRIS providers without rewriting integrations. |
| HR Analytics | Aggregate people data from multiple HR systems for workforce analytics. |

## Integrations

| Name | Description |
|------|-------------|
| BambooHR | Sync employee data from BambooHR via Bindbee unified API. |
| Workday | Access Workday employee and org data through normalized Bindbee API. |
| ADP | Connect ADP Workforce Now employee records via Bindbee. |
| Greenhouse | Access Greenhouse ATS job listings and candidates via Bindbee. |
| Lever | Sync Lever ATS recruiting pipeline data through Bindbee. |

## Artifacts

### OpenAPI

- [Bindbee API](openapi/bindbee-api.yaml)

### JSON Schema

- [bindbee-employee-schema.json](json-schema/bindbee-employee-schema.json)
- [bindbee-department-schema.json](json-schema/bindbee-department-schema.json)
- [bindbee-time-off-request-schema.json](json-schema/bindbee-time-off-request-schema.json)
- [bindbee-job-schema.json](json-schema/bindbee-job-schema.json)
- [bindbee-candidate-schema.json](json-schema/bindbee-candidate-schema.json)

## Capabilities

### Shared Per-API Definitions

- [Bindbee API](capabilities/shared/bindbee.yaml) — 7 operations for employees, departments, time-off, jobs, and candidates

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [HR Integration](capabilities/hr-integration.yaml) | Bindbee | 7 | HR Developer, People Ops Engineer |

## Vocabulary

- [Bindbee Vocabulary](vocabulary/bindbee-vocabulary.yaml) — Unified taxonomy mapping 5 resources, 2 actions, 1 workflow, and 2 personas

## Rules

- [Bindbee Spectral Rules](rules/bindbee-spectral-rules.yml) — 24 rules across 9 categories enforcing Bindbee API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
