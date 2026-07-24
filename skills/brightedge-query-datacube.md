---
name: Query the DataCube for top pages and keywords
description: Use the dataset endpoints to retrieve top pages for a domain and top
  keywords per page or per domain.
api: openapi/brightedge-platform-openapi-original.json
operations:
- ping_5_0_dataset_ping_get
- get_top_pages_5_0_dataset_top_pages_post
- get_top_keywords_per_domain_5_0_dataset_top_keywords_post
- get_top_keywords_per_page_5_0_dataset_top_keywords_per_page_post
- crawl_page_content_5_0_dataset_page_crawl_post
---

# Query the DataCube for top pages and keywords

Use the dataset endpoints to retrieve top pages for a domain and top keywords per page or per domain.

## Steps

1. Authenticate as above.
2. Optionally call `ping_5_0_dataset_ping_get` to confirm dataset availability.
3. POST to `get_top_pages_5_0_dataset_top_pages_post` with a base domain to retrieve the top N pages (default 1000).
4. POST to `get_top_keywords_per_domain_5_0_dataset_top_keywords_post` for the top keywords for a domain, or `get_top_keywords_per_page_5_0_dataset_top_keywords_per_page_post` for a single page URL.
5. Use `crawl_page_content_5_0_dataset_page_crawl_post` to fetch crawled page HTML for a URL. Requests are JSON bodies; malformed input returns HTTP 422.

All operationIds above are verified against openapi/brightedge-platform-openapi-original.json. Cross-references: conventions/brightedge-conventions.yml, errors/brightedge-problem-types.yml, authentication/brightedge-authentication.yml.
