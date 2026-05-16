# API Testing Portfolio

API testing practice suite built with Postman and (coming: pytest / Playwright).

## What's in here
- `postman/` — Postman collection targeting Reqres.in
  - 6 requests: GET list, GET single, GET 404, POST create, PUT update, DELETE
  - Tests on every request: status code, response time, body schema
  - Chained requests using collection variables
  - Environment file with configurable baseUrl
-  docs/security-checklist.md — auth and security test checklist

## How to run
1. Import `postman/reqres-collection.json` into Postman
2. Import `postman/reqres-env.json` and select it
3. Click the collection → Run collection → Run

## Stack
- Postman (manual + automated collection runner)
- Newman (CLI runner) — coming Day 5
- pytest / Playwright API tests — coming Day 4
