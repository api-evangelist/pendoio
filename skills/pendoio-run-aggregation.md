---
name: Query product analytics with the Aggregation API
description: Authenticate to Pendo and run a structured Aggregation query to pull product-usage analytics.
api: openapi/pendoio-engage-openapi.yml
operations: [verifyIntegrationKey, returnListOfAllPages, returnListOfAllFeatures, visitors]
---

# Query product analytics with the Pendo Aggregation API

Use this to read product-engagement data (page/feature usage, visitor activity) from Pendo.

## Auth
- Send `x-pendo-integration-key: <KEY>` on every request. The key is created in
  Pendo Settings > Integrations > Integration Keys.
- Base URL: `https://app.pendo.io` (use the regional host for EU/AU/JP subscriptions).

## Steps
1. **Verify the key** — `GET /api/v1/token/verify` (`verifyIntegrationKey`). Stop if not 200.
2. **Resolve the objects you want to measure** — `GET /api/v1/page` (`returnListOfAllPages`)
   and/or `GET /api/v1/feature` (`returnListOfAllFeatures`) to get page/feature ids.
3. **Run the aggregation** — `POST /api/v1/aggregation` (`visitors`) with a MongoDB-like
   `request.pipeline` body. Sources include `pageEvents`, `featureEvents`, `events`,
   `visitors`, `accounts`; chain `filter`, `group`, `select`, and time windows.

## Conventions & errors
- Aggregation is the paging/windowing mechanism (no cursor pagination on list endpoints);
  see `conventions/pendoio-conventions.yml`.
- Keep the integration key server-side — never expose it in client JS.
- 403 = key lacks the required access level or API access is not enabled for the
  subscription. See `errors/pendoio-problem-types.yml`.
