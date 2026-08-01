---
name: Build a segment and target an in-app guide
description: Create a Pendo segment, add visitors to it, and point an in-app guide at that segment.
api: openapi/pendoio-engage-openapi.yml
operations: [createASegment, addAVisitorToASegment, returnListOfAllGuides, changeGuideSegment, changeGuideState]
---

# Build a segment and target an in-app guide

## Auth
- Header `x-pendo-integration-key: <KEY>`; base URL `https://app.pendo.io`.

## Steps
1. **Create a segment** — `POST /api/v1/segment/upload` or `createASegment`
   (`POST /api/v1/segment`) with the segment definition.
2. **Populate it** — add visitors one at a time with
   `PUT /api/v1/segment/:segmentId/visitor/:visitorId` (`addAVisitorToASegment`),
   or batch with `PATCH /api/v1/segment/:segmentId/visitor`.
3. **Find the guide** — `GET /api/v1/guide` (`returnListOfAllGuides`) to get the `guideId`.
4. **Target the guide at the segment** — `PUT /api/v1/guide/:guideId/segment`
   (`changeGuideSegment`).
5. **Activate** — `PUT /api/v1/guide/:guideId/state` (`changeGuideState`) to set the
   guide to `public`/`staged`.

## Conventions & errors
- Mutations are id-addressed and idempotent by resource id (no idempotency-key header;
  see `conventions/pendoio-conventions.yml`).
- 404 if the segment or guide id does not exist; 403 on insufficient key access.
