---
name: Issue and activate an access card
description: Create a physical/mobile card, assign it to a user, and activate the assignment so it opens doors.
api: openapi/kisi-openapi-original.yml
operations: [fetchUsers, createCard, createCardAssignment, activateCardAssignment, fetchCards]
---

# Issue and activate an access card

Provision a Kisi card credential and bind it to a user.

## Preconditions
- Base URL `https://api.kisi.io`, JSON only, `Authorization` header auth (`401` if missing).
- 5 req/s per-user rate limit; `429` on exceed.

## Steps
1. `fetchUsers` — locate the target user and capture `user_id`.
2. `createCard` — create the card credential (physical badge or mobile) and capture the card id.
3. `createCardAssignment` — assign the card to the user (and place/group as applicable). Capture the assignment id.
4. `activateCardAssignment` — activate the assignment so the credential is live. (Token-based activation is available via `activateCardAssignmentWithActivationToken`.)
5. `fetchCards` — confirm the card appears and is active.

## Error handling
- `faa9ff` The card is not activated, `faa9ef` The card was not found, `cabbeb` a card with the same identifiers was already enrolled — inspect and resolve before retrying (`errors/kisi-error-codes.yml`).
- Deactivate with `deactivateCardAssignment`; delete with `deleteCard`.

Cross-reference: `data-model/kisi-data-model.yml`, `conventions/kisi-conventions.yml`.
