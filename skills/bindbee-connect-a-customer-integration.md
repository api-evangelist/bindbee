---
name: bindbee-connect-a-customer-integration
description: Connect one of your customers to their HRIS, ATS or LMS through Bindbee Embed and obtain the connector token that authorizes every later data call for that customer.
api: Bindbee Embedded API
operations:
  - list_integrations_api_v1_lookup_integrations_get
  - create_link_token_api_embedded_v1_link_create_link_token_post
  - get_connector_token_api_embedded_v1_connectors_connector_token__temporary_token__get
  - get_hris_connectors_api_hris_v1_connectors_get
  - force_resync_connector_api_embedded_v1_connectors_resync_post
generated: '2026-09-04'
method: generated
source: openapi/_original/bindbee-openapi.json + https://docs.bindbee.dev/sdk/bindbee-embed
---

# Connect a customer integration with Bindbee Embed

Bindbee is a unified API: you integrate once, and each of your customers connects their own HR
system. Nothing you do against employee, candidate or course data works until that customer has a
**connector**, and a connector is represented by a **connector token**. This skill gets you one.

## Before you start

- Base URL is `https://api.bindbee.dev` (EU tenants: `https://api-eu.bindbee.dev`).
- Every call carries `Authorization: Bearer <BINDBEE_API_KEY>`.
- The **environment is chosen by the key**. A Development API key creates a Development connector;
  a Production API key creates a Production connector. The two are completely isolated — a token
  minted in one will not read data in the other.

## Steps

1. **Find the integration slug.** `GET /api/v1/lookup/integrations`
   (`list_integrations_api_v1_lookup_integrations_get`) returns the connectable systems and the
   `category` / `integration` values the link-token call expects (for example category `HRIS`,
   integration `bamboohr`). Do not hand-type a slug — read it from this response.

2. **Mint a link token.** `POST /api/embedded/v1/link/create-link-token`
   (`create_link_token_api_embedded_v1_link_create_link_token_post`) with the customer's
   `end_user_data` (`org_name` plus your own stable `origin_id` for that organization), the
   `category` and the `integration`. The response carries a `link_token` that initialises one
   Bindbee Embed session.

3. **Run the Embed flow in your frontend.** Load `@bindbee/react-link` from npm, or the JavaScript
   build at `https://cdn.bindbee.dev/initialize.min.js`, and hand it the `link_token`. The customer
   authenticates against their own HR system inside your product. On success the widget returns a
   `temporary_token`.

4. **Exchange it.** `GET /api/embedded/v1/connectors/connector_token/{temporary_token}`
   (`get_connector_token_api_embedded_v1_connectors_connector_token__temporary_token__get`)
   returns the durable `connector_token`. **Store this per customer** — it is the credential every
   subsequent unified-API read and write uses.

5. **Call the unified API.** From now on send both headers together:
   `Authorization: Bearer <BINDBEE_API_KEY>` and `X-Connector-Token: <connector_token>`.
   Confirm the connector is live with `GET /api/hris/v1/connectors`
   (`get_hris_connectors_api_hris_v1_connectors_get`), which reports its sync status.

6. **Force a refresh when you need one.** Bindbee syncs from the upstream system once every 24
   hours by default. `POST /api/embedded/v1/connectors/resync`
   (`force_resync_connector_api_embedded_v1_connectors_resync_post`) pulls sooner. Data written
   through Bindbee is passed to the third-party system; data read is served from Bindbee's last
   sync, so a read straight after a write may not reflect it until the next sync.

## Rules that apply to every call

- **Rate limit: 200 requests per minute per connector token**, not per API key. Read
  `X-RateLimit-Remaining`; on `429` wait for `Retry-After` seconds. Different customers have
  independent budgets, so parallelising across connectors is the supported way to go faster.
- **Pagination** is cursor-based: `cursor` and `page_size` query parameters.
- **No idempotency key exists.** No Bindbee write operation accepts an `Idempotency-Key` header, so
  a retried `POST` after a timeout can create a duplicate record upstream. Guard writes on your own
  side with your `origin_id` / `remote_id` correlation before retrying.
- **Errors**: `422` is FastAPI request validation (a `detail[]` array of `loc`/`msg`/`type`), not a
  business error. `429` is rate limiting.

## Deleting a connector is irreversible

`DELETE /api/{hris|ats|lms}/v1/connectors/{connector_id}/delete` removes the connector. Bindbee
publishes no undelete, no restore window and no soft-delete. The customer must run the Embed flow
again from step 2, and every stored connector token for them becomes useless.
