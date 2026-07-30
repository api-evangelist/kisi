---
name: Onboard a member and grant door access
description: Invite a person to a Kisi place, add them to an access group, and confirm they can unlock the door.
api: openapi/kisi-openapi-original.yml
operations: [fetchPlaces, fetchGroups, createMember, createGroupLock, unlockLock]
---

# Onboard a member and grant door access

Use the Kisi API to bring a new person online with access to a specific door.

## Preconditions
- Base URL: `https://api.kisi.io`. JSON only — set `Accept: application/json` and `Content-Type: application/json`.
- Authenticate with the `Authorization` header (Kisi-Login or Kisi-Access-Key). Missing/invalid auth returns `401`.
- Respect the rate limit: 5 requests/second per user; wait ~1s between calls and issue single-user requests serially.

## Steps
1. `fetchPlaces` — list the places (buildings) and pick the target `place_id`.
2. `fetchGroups` — list access groups for that place; choose the group that maps to the doors you want to grant.
3. `createMember` — add the person to the place by email; this triggers a Kisi invite. Capture the returned member id.
4. `createGroupLock` — ensure the chosen group is linked to the target lock/door (if not already), so group members inherit access.
5. `unlockLock` — verify by unlocking the lock as an authorized user; a successful call confirms the access path.

## Error handling
- Errors return `{ "code": "...", "error": "..." }` (see `errors/kisi-error-codes.yml`). Validation errors use `{ "errors": { field: [...] } }` (422).
- `403`/`fcd8ef` Access denied, `faa9ef` card not found, `fad10e` door not found — resolve group/place membership before retrying.
- On `429`, back off at least one second and retry serially.

Cross-reference: `conventions/kisi-conventions.yml`, `authentication/kisi-authentication.yml`, `errors/kisi-error-codes.yml`.
