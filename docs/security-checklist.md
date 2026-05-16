# API security test checklist

## Authentication checks
- [ ] No token → 401 (not 200, not 403, not 500)
- [ ] Invalid/random token string → 401
- [ ] Malformed token (e.g. "Bearer " with nothing after) → 401
- [ ] Expired token → 401 with clear error message
- [ ] Correct token format but signed with wrong secret → 401

## Authorisation checks
- [ ] User A cannot GET user B's private resources → 403
- [ ] User A cannot PATCH/DELETE user B's resources → 403
- [ ] Non-admin user cannot access admin endpoints → 403
- [ ] Role escalation: send isAdmin:true in body → server ignores it

## Data exposure checks
- [ ] Response does not include password, passwordHash, or secret fields
- [ ] Response does not include internal database IDs not meant for clients
- [ ] Error messages do not expose stack traces or internal paths

## Input handling
- [ ] Extra/unexpected fields in POST body are ignored
- [ ] Very large payloads are rejected with 413 (not crash)

## Rate limiting
- [ ] Rapid repeated requests eventually return 429
- [ ] 429 response includes Retry-After header
