---
name: Pull keyword rankings for an account
description: Discover the accounts a user can access, then list the account keywords
  and keyword groups to pull ranking data.
api: openapi/brightedge-platform-openapi-original.json
operations:
- get_all_accounts_5_0_objects_accounts_get
- get_account_5_0_objects_accounts__account_id__post
- get_account_keywords_5_0_objects_keywords__account_id__get
- get_account_keywordgroups_5_0_objects_keywordgroups__account_id__get
- get_account_keywordgroup_detail_5_0_objects_keywordgroups__account_id___keyword_group_id__get
---

# Pull keyword rankings for an account

Discover the accounts a user can access, then list the account keywords and keyword groups to pull ranking data.

## Steps

1. Authenticate with one of the supported schemes: HTTP Basic, an `X-Token` header, a `Bearer-Token` header, or a `X-BRIGHTEDGE-SESSION` header / `BRIGHTEDGE` cookie (see authentication/brightedge-authentication.yml).
2. Call `get_all_accounts_5_0_objects_accounts_get` to list the accounts the authenticated user can access; capture the target `account_id`.
3. Optionally call `get_account_5_0_objects_accounts__account_id__post` for account details.
4. Call `get_account_keywordgroups_5_0_objects_keywordgroups__account_id__get` to list keyword groups, then `get_account_keywords_5_0_objects_keywords__account_id__get` (or `get_account_keywordgroup_detail_...` for one group) to pull the keywords and ranking data.
5. Page large result sets with `offset` + `limit` (use `include_total`/`only_total` to get totals). Validation failures return HTTP 422 with a `detail[]` array (errors/brightedge-problem-types.yml).

All operationIds above are verified against openapi/brightedge-platform-openapi-original.json. Cross-references: conventions/brightedge-conventions.yml, errors/brightedge-problem-types.yml, authentication/brightedge-authentication.yml.
