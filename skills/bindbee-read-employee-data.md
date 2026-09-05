---
name: bindbee-read-employee-data
description: Read normalized employee, employment, compensation and time-off records out of any connected HRIS through the Bindbee unified API, paging correctly and reaching fields the unified model does not cover.
api: Bindbee HRIS API
operations:
  - get_employees_api_hris_v1_employees_get
  - get_employee_by_id_api_hris_v1_employees__id__get
  - get_time_off_list_api_hris_v1_time_off_get
  - get_time_off_balances_list_api_hris_v1_time_off_balances_get
  - list_custom_fields_api_v1_custom_fields_get
  - make_passthrough_request_api_v1_passthrough_post
generated: '2026-09-04'
method: generated
source: openapi/_original/bindbee-openapi.json + https://docs.bindbee.dev/api-reference/basics/pagination
---

# Read employee data from any connected HRIS

One shape of request works across every system Bindbee connects — BambooHR, Workday, ADP, Hibob,
Personio and the rest. You do not branch on the vendor.

## Headers on every request

```
Authorization: Bearer <BINDBEE_API_KEY>
X-Connector-Token: <connector_token for this customer>
```

Get a connector token first — see `bindbee-connect-a-customer-integration`.

## Steps

1. **List employees.** `GET /api/hris/v1/employees`
   (`get_employees_api_hris_v1_employees_get`). Useful filters, all optional and all real
   parameters on the operation: `ids`, `manager_id`, `company_id`, `remote_id`,
   `employment_status`, `include_raw_data`, `include_custom_fields`.

2. **Page with the cursor, never with an offset.** Pass `page_size` and then feed the cursor the
   response returns back as `cursor`. Stop when no next cursor comes back. Do not compute page
   numbers — the API has no offset parameter.

3. **Fetch one record** with `GET /api/hris/v1/employees/{id}`
   (`get_employee_by_id_api_hris_v1_employees__id__get`). The `id` is Bindbee's normalized UUID;
   the upstream system's own identifier is `remote_id`, and you can look an employee up by it with
   the `remote_id` filter on the list call.

4. **Pull time off alongside it.** `GET /api/hris/v1/time-off`
   (`get_time_off_list_api_hris_v1_time_off_get`) for requests and
   `GET /api/hris/v1/time-off-balances`
   (`get_time_off_balances_list_api_hris_v1_time_off_balances_get`) for accrued balances. Both
   filter by employee.

5. **When the unified model does not carry the field you need**, you have two escapes, in this
   order:
   - **Custom fields.** `GET /api/v1/custom-fields`
     (`list_custom_fields_api_v1_custom_fields_get`) plus the mapping endpoints let you promote an
     integration-specific attribute into the unified model with a JMESPath expression. Then ask for
     it with `include_custom_fields=true`.
   - **Passthrough.** `POST /api/v1/passthrough`
     (`make_passthrough_request_api_v1_passthrough_post`) forwards a raw request to the underlying
     HR system through the connector's credentials. This is vendor-specific by definition: what you
     send and what you get back is that vendor's own API, so anything built on it stops being
     portable across connectors.
   - `include_raw_data=true` on any read returns the untouched upstream payload beside the
     normalized one, which is the cheapest way to see what a field is actually called upstream.

## What to expect operationally

- **Freshness:** reads are served from Bindbee's synced copy, refreshed every 24 hours by default
  (12–24 hours on Pro, configurable on Enterprise). This is not a live pass-through read. If you
  need current data, force a resync first, or subscribe to the sync-completed and
  employee-model-change webhooks and read on the event.
- **Rate limit:** 200 requests per minute per connector token. Honour `Retry-After` on `429`.
- **Errors:** `422` carries FastAPI's validation `detail[]` array — a malformed query parameter,
  not a missing employee. A missing connector token surfaces as an authorization failure, not a
  `404`.
- **These operations are all reads.** Nothing here mutates the customer's HR system.
