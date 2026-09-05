---
name: Bindbee
description: Use when building integrations with HRIS, Payroll, ATS, or LMS systems. Reach for this skill when you need to read employee data, create records, manage connectors, configure custom fields, set up webhooks, or work with unified APIs across multiple third-party platforms.
metadata:
    mintlify-proj: bindbee
    version: "1.0"
---

# Bindbee Skill

## Product Summary

Bindbee is a unified API platform that enables B2B SaaS companies to integrate with 100+ HRIS, Payroll, ATS, and LMS systems without building 1-on-1 integrations. It provides standardized endpoints for reading and writing data across these categories, automatic data synchronization via connectors, custom field extensions, and webhook-based event notifications.

**Key files and concepts:**
- **API Base URL:** `https://api.bindbee.dev`
- **Categories:** HRIS, ATS, LMS (each has its own API namespace: `/api/{category}/v1/`)
- **Authentication:** Bearer token (API key) for organization-level operations; connector tokens for end-user data access
- **Connectors:** Bridges between your app and a customer's third-party tool; each connector has a unique token and sync status
- **Unified Models:** Standardized schemas (employee, candidate, job, course, etc.) that normalize data across different integrations
- **Custom Fields:** Extend unified models with integration-specific attributes using JMESPath expressions

**Primary documentation:** https://docs.bindbee.dev

## When to Use

Reach for this skill when:
- Building integrations with customer HRIS/Payroll systems (Workday, BambooHR, Gusto, etc.)
- Fetching employee, payroll, or compensation data via unified API
- Creating or updating records (employees, candidates, time off, payroll runs)
- Managing connectors and monitoring sync status
- Extending unified models with custom fields for integration-specific data
- Setting up webhooks to receive real-time sync and data change notifications
- Debugging connector issues or inspecting raw upstream payloads
- Handling rate limits or pagination for bulk data operations

## Quick Reference

### Authentication Headers

| Use Case | Header | Format |
|----------|--------|--------|
| Organization-level (list connectors, manage custom fields) | `Authorization` | `Bearer YOUR_API_KEY` |
| End-user data access (read/write employee data) | `X-Connector-Token` | `END_USER_CONNECTOR_TOKEN` |
| Idempotent writes | `X-Idempotency-Key` | Any unique string (UUID recommended) |

### API Endpoints by Category

| Category | Base URL | Common Models |
|----------|----------|----------------|
| HRIS | `/api/hris/v1/` | employees, companies, compensation, benefits, payroll_runs, time_off |
| ATS | `/api/ats/v1/` | candidates, jobs, applications, offers, interviews, departments |
| LMS | `/api/lms/v1/` | users, courses, enrollments, completions, skills |

### Common Query Parameters

| Parameter | Purpose | Example |
|-----------|---------|---------|
| `page_size` | Results per page (max 200) | `?page_size=50` |
| `cursor` | Pagination cursor | `?cursor=abc123` |
| `include_custom_fields` | Include custom field values | `?include_custom_fields=true` |
| `include_raw_data` | Include original third-party payload | `?include_raw_data=true` |
| `modified_after` | Filter by sync date (ISO 8601) | `?modified_after=2024-01-01T00:00:00Z` |

### Connector Status Values

| Status | Meaning |
|--------|---------|
| `COMPLETE` | Connector is linked and ready |
| `INCOMPLETE` | Connector setup not finished |
| `RELINK_NEEDED` | Credentials expired; user must re-authorize |

### Sync Status Values

| Status | Meaning |
|--------|---------|
| `Syncing` | Sync in progress |
| `Done` | Sync completed successfully |
| `Failed` | Sync encountered an error |

## Decision Guidance

### When to Use Organization-Scoped vs Connector-Scoped Custom Field Mappings

| Scenario | Use Organization-Scoped | Use Connector-Scoped |
|----------|------------------------|----------------------|
| Field mapping is consistent across all connectors of an integration | ✓ | |
| Need to override mapping for a specific customer's connector | | ✓ |
| Setting up fields for the first time across an integration | ✓ | |
| Handling integration-specific quirks for one customer | | ✓ |

**Rule:** Connector-scoped mappings override organization-scoped ones. Always check existing org-level mappings before creating connector-level duplicates.

### When to Use Dashboard vs API for Custom Fields

| Task | Dashboard | API |
|------|-----------|-----|
| Ad-hoc field setup and testing | ✓ | |
| Exploring upstream payloads interactively | ✓ | |
| Automating field provisioning across environments | | ✓ |
| Infrastructure-as-code / version control | | ✓ |
| One-off field changes | ✓ | |
| Replicating config across dev/staging/prod | | ✓ |

### When to Use Passthrough API vs Unified Endpoints

| Scenario | Use Unified Endpoints | Use Passthrough |
|----------|----------------------|-----------------|
| Reading standard employee/candidate data | ✓ | |
| Writing to supported models (employee, time off, etc.) | ✓ | |
| Accessing integration-specific fields not in unified schema | | ✓ |
| Calling integration APIs not yet wrapped by Bindbee | | ✓ |

## Workflow

### Reading Data from a Connector

1. **Obtain the connector token** — retrieve from connector details in dashboard or via `GET /api/{category}/v1/connectors`
2. **Check connector status** — ensure `status: "COMPLETE"` and `sync_status: "Done"` before querying
3. **Construct the request** — use `GET /api/{category}/v1/{model}` with `X-Connector-Token` header
4. **Handle pagination** — use `cursor` and `page_size` for large datasets; max page size is 200
5. **Include custom fields if needed** — add `?include_custom_fields=true` to response
6. **Check rate limits** — monitor `X-RateLimit-Remaining` header; 200 requests/minute per connector

### Creating a Record (Employee, Candidate, etc.)

1. **Fetch the meta schema** — call `GET /api/{category}/v1/{model}/meta/post` with connector token to see required fields
2. **Review integration_params** — some integrations require specific parameters (e.g., pay statement type ID)
3. **Construct request body** — include all required fields; use `additional_attributes` for integration-specific data
4. **Add idempotency key** — include `X-Idempotency-Key` header to prevent duplicate creates on retry
5. **Submit POST request** — `POST /api/{category}/v1/{model}` with `X-Connector-Token` header
6. **Validate response** — check for 200/201 status and returned object ID

### Setting Up Custom Fields

1. **Discover available models** — `GET /api/v1/lookup/models?category=HRIS`
2. **Inspect raw payload** — `GET /api/v1/custom-fields/raw-data?category=HRIS&model=employee&integration_slug=workday`
3. **Validate JMESPath** — `POST /api/v1/custom-fields/preview` with test expression (optional but recommended)
4. **Create the field** — `POST /api/v1/custom-fields` with name, category, model
5. **Create mapping** — `POST /api/v1/custom-fields/mapping` with either `integration_slug` (org-scoped) or `connector_token` (connector-scoped)
6. **Verify configuration** — `GET /api/v1/custom-fields/configuration?connector_token=...` to see effective mappings

### Monitoring Connector Syncs via Webhooks

1. **Create webhook** — navigate to Webhooks in dashboard; set destination URL and select events
2. **Choose events** — select `connector_sync_started`, `connector_synced`, `connector_sync_error`, `employee_data_changed`, or `connector_data_modified`
3. **Verify signature** — check `X-Bindbee-Webhook-Signature` header using HMAC-SHA256 with your org's signature key
4. **Handle retries** — Bindbee retries failed webhooks up to 4 times within 60 seconds; respond with 2xx within 10 seconds
5. **Parse payload** — extract `connector`, `data`, `sync`, and `error` (if applicable) objects

## Common Gotchas

- **Missing connector token on data requests:** All endpoints that access end-user data require `X-Connector-Token` header. Organization-level endpoints (custom fields, connectors list) use `Authorization: Bearer` only.
- **Connector status not COMPLETE:** Connectors in `INCOMPLETE` or `RELINK_NEEDED` status cannot be queried. Check status before making data requests.
- **Rate limit per connector, not per API key:** Each connector has its own 200 req/min limit. Different end users don't share limits, but a single connector will be throttled if you exceed 200 req/min.
- **Invalid JMESPath in custom fields:** If a JMESPath expression is invalid or fails to resolve, the response returns `"INVALID_JSON_PATH"` instead of raising an error. Always validate expressions via the preview endpoint before saving.
- **Connector-scoped mappings override org-scoped:** If both exist for the same field, the connector-scoped mapping wins. Check existing org mappings before creating connector-level overrides.
- **Meta API not implemented for all integrations:** Some write operations (e.g., Time Off in BambooHR) may not have Meta API support yet. Check for 501 response and contact support if needed.
- **Development connectors don't auto-sync:** Connectors created with dev API keys require manual sync triggers via dashboard or `POST /api/{category}/v1/connectors/{id}/resync`. Production connectors sync automatically every 24 hours.
- **Sync frequency is 24 hours by default:** Data is refreshed once per day unless customized. Don't expect real-time updates from third-party systems.
- **Pagination cursor is opaque:** Don't try to parse or construct cursor values; treat them as opaque strings returned by the API.
- **Webhook signature verification is required:** Always validate the `X-Bindbee-Webhook-Signature` header to ensure requests are genuinely from Bindbee.

## Verification Checklist

Before submitting work with Bindbee integrations:

- [ ] API key and connector tokens are stored securely (not in code/logs)
- [ ] All requests include correct authentication headers (`Authorization` or `X-Connector-Token`)
- [ ] Connector status is `COMPLETE` and sync status is `Done` before querying
- [ ] Rate limit headers are monitored; backoff strategy implemented for 429 responses
- [ ] Pagination is handled correctly for large datasets (use cursor, not offset)
- [ ] Custom field JMESPath expressions are validated via preview endpoint before saving
- [ ] Write operations include `X-Idempotency-Key` header to prevent duplicates
- [ ] Meta API schema is fetched and validated before constructing write requests
- [ ] Webhook signature is verified using HMAC-SHA256 with org signature key
- [ ] Error responses are logged with full payload for debugging
- [ ] Integration-specific fields are handled via `additional_attributes` or passthrough API
- [ ] `include_custom_fields=true` is added to requests when custom fields are needed

## Resources

**Comprehensive page-by-page navigation:** https://docs.bindbee.dev/llms.txt

**Critical documentation pages:**
- [Features Overview](https://docs.bindbee.dev/features/overview) — high-level product capabilities
- [API Authentication](https://docs.bindbee.dev/api-reference/basics/authentication) — API key and connector token usage
- [Custom Fields API Workflow](https://docs.bindbee.dev/custom-fields/api-workflow) — step-by-step guide for extending unified models

---

> For additional documentation and navigation, see: https://docs.bindbee.dev/llms.txt