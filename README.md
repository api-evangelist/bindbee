# Bindbee (bindbee)
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
