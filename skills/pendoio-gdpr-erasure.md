---
name: Erase visitor data for GDPR/CCPA
description: Submit and track a bulk deletion of Pendo visitor data to satisfy a GDPR/CCPA erasure request.
api: openapi/pendoio-engage-openapi.yml
operations: [verifyIntegrationKey, bulkDeleteVisitors, getAllOutstandingBulkDeletionRequests, getOneBulkDeletionStatus]
---

# Erase visitor data for GDPR/CCPA

## Auth
- Header `x-pendo-integration-key: <KEY>`; base URL `https://app.pendo.io`.

## Steps
1. **Verify the key** — `GET /api/v1/token/verify` (`verifyIntegrationKey`).
2. **Submit the deletion** — `POST /api/v1/bulkdelete/visitor` (`bulkDeleteVisitors`)
   with the list of visitor ids to erase. (Use `POST /api/v1/bulkdelete/account`
   for accounts.)
3. **Track outstanding jobs** — `GET /api/v1/bulkdelete`
   (`getAllOutstandingBulkDeletionRequests`).
4. **Check one job** — `GET /api/v1/bulkdelete/:id` (`getOneBulkDeletionStatus`)
   until the erasure completes.

## Notes
- This is the right-to-erasure path for GDPR/CCPA; deletion is asynchronous and
  irreversible. Confirm the id list before submitting.
- 403 on insufficient key access; see `errors/pendoio-problem-types.yml`.
