---
name: Create and retrieve a bulk export job
description: Kick off an asynchronous bulk export job and poll for the results by
  job/batch id.
api: openapi/brightedge-platform-openapi-original.json
operations:
- create_bulk_export_job_5_0_jobs_post
- get_bulk_export_jobs_by_batch_id_5_0_jobs__job_id__get
- get_account_bulk_export_jobs_5_0_jobs_account__account_id__get
---

# Create and retrieve a bulk export job

Kick off an asynchronous bulk export job and poll for the results by job/batch id.

## Steps

1. Authenticate as above.
2. POST to `create_bulk_export_job_5_0_jobs_post` to create one or more bulk export jobs; capture the returned batch/job id.
3. Poll `get_bulk_export_jobs_by_batch_id_5_0_jobs__job_id__get` with the job/batch id until the job completes.
4. List all jobs for an account with `get_account_bulk_export_jobs_5_0_jobs_account__account_id__get`.
5. Large exports may be returned as `text/csv`. Treat export generation as asynchronous and back off between polls.

All operationIds above are verified against openapi/brightedge-platform-openapi-original.json. Cross-references: conventions/brightedge-conventions.yml, errors/brightedge-problem-types.yml, authentication/brightedge-authentication.yml.
