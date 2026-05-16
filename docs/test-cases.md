# API test cases — POST /users

Endpoint: POST {{baseUrl}}/users  
Tool: Reqres.in (demo API)  
Framework: 5-category test design  

---

## Category 1 — Happy path

| ID | Description | Input | Expected result |
|----|-------------|-------|-----------------|
| TC01 | Valid name and job | `{ "name": "Jane Test", "job": "QA Engineer" }` | 201, response has id (string), name and job match input, createdAt is valid timestamp |
| TC02 | Name with special characters | `{ "name": "María José", "job": "QA" }` | 201, name stored correctly without character mangling |
| TC03 | Long but valid name (100 chars) | `{ "name": "Aaaa...100 chars", "job": "QA" }` | 201 if within length limits |

---

## Category 2 — Negative / error cases

| ID | Description | Input | Expected result |
|----|-------------|-------|-----------------|
| TC04 | Missing name field | `{ "job": "QA Engineer" }` | 400 or 422 with error mentioning 'name' |
| TC05 | Missing job field | `{ "name": "Jane Test" }` | 400 or 422 with error mentioning 'job' |
| TC06 | Empty body | `{}` | 400 or 422 |
| TC07 | Name is null | `{ "name": null, "job": "QA" }` | 400 or 422 — null is different from missing |
| TC08 | Name is a number | `{ "name": 123, "job": "QA" }` | 400 — wrong type |
| TC09 | Name is an array | `{ "name": ["Jane"], "job": "QA" }` | 400 — wrong type |
| TC10 | Name is empty string | `{ "name": "", "job": "QA" }` | 400 or 422 — empty string is not a valid name |

> **Note:** Reqres does not enforce required fields or types. All TC04–TC10 return 201 on Reqres.
> These cases document expected behaviour on a real API, not Reqres behaviour.

---

## Category 3 — Boundary values

| ID | Description | Input | Expected result |
|----|-------------|-------|-----------------|
| TC11 | Name is 1 character | `{ "name": "J", "job": "QA" }` | 201 (unless minimum length enforced) |
| TC12 | Name is 255 characters | `{ "name": "Aaa...255 chars", "job": "QA" }` | 201 |
| TC13 | Name is 256 characters | `{ "name": "Aaa...256 chars", "job": "QA" }` | 422 if 255 is max limit — document actual result |
| TC14 | Name is only spaces | `{ "name": "   ", "job": "QA" }` | 422 — whitespace-only should be rejected |

---

## Category 4 — Business logic

| ID | Description | Input | Expected result |
|----|-------------|-------|-----------------|
| TC15 | Create same user twice | Same body sent twice | 409 Conflict on second request, or 201 if duplicates allowed — document which |
| TC16 | Mass assignment attempt | `{ "name": "Jane", "job": "QA", "isAdmin": true, "role": "superuser" }` | 201 but response must NOT contain isAdmin or role fields |
| TC17 | Extremely large payload | name field = 10MB string | 413 Payload Too Large — server must not crash |

---

## Category 5 — Schema / contract

| ID | Description | What to check | Expected result |
|----|-------------|---------------|-----------------|
| TC18 | Response has all expected fields | id, name, job, createdAt present | All 4 fields always present, no missing fields |
| TC19 | Field types are correct | id is string, name is string, job is string, createdAt is string | No type mismatches |
| TC20 | createdAt is valid ISO 8601 | Parse createdAt with new Date() | Must not return 'Invalid Date' |

---

## Test run notes

- **Tool:** Postman collection (see /postman/reqres-collection.json)
- **Reqres limitations:** Does not enforce required fields, types, or uniqueness. TC04–TC10 and TC15 cannot be properly validated against Reqres. These cases would be executed against a real API in a project context.
- **Automated:** TC01, TC16, TC18, TC19, TC20 have running Postman tests
- **Manual / documented only:** TC02, TC03, TC04–TC15, TC17
