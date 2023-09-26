---
title: "Update: Improvements to dbt Cloud continuous integration"
description: "September 2023: Improved deletion of temporary schemas"
sidebar_label: "Update: Improved automatic deletion of temporary schemas"
tags: [Sept-2023, CI]
date: 2023-09-18
sidebar_position: 08
---

Temporary schemas are now being automatically deleted (dropped) for all adapters (like Databricks), PrivateLink connections, and environment variables in connection strings. 

dbt Labs has rearchitected how schema deletion works for [continuous integration (CI)](/docs/deploy/continuous-integration) runs. We created a new service to delete any schema with a prefix of `dbt_cloud_pr_` that's been generated by a PR run.

However, temporary schemas will not be automatically deleted if:
- Your project overrides the [generate_schema_name macro](/docs/build/custom-schemas) but it doesn't contain the required prefix `dbt_cloud_pr_`. For details, refer to [Troubleshooting](/docs/deploy/ci-jobs#troubleshooting).
- You're using a [non-native Git integration](/docs/deploy/ci-jobs#trigger-a-ci-job-with-the-api). This is because automatic deletion relies on incoming webhooks from Git providers, which is only available through the native integrations.