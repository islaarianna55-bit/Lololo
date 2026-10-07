# SanctionsKit synthetic screening and evidence

This starter screens the fictional person Alex Example and retrieves evidence for the same result. It uses the free synthetic sandbox, not live sanctions data. Production screening requires paid access and separate production configuration.

## Requirements

- Self-hosted n8n with the `n8n-nodes-sanctionskit` community package version `0.1.0` installed.
- A SanctionsKit workspace and a sandbox API key. The key needs `sources:read` for credential testing, `screenings:write` to create a screening, and `results:read` to retrieve its evidence.

The template does not depend on availability in the n8n Cloud verified-node catalog.

## Run the example

1. Install `n8n-nodes-sanctionskit` in your self-hosted n8n instance using its community-node installation flow.
2. Import the adjacent `SanctionsKit synthetic screening and evidence.json` file.
3. Create a **SanctionsKit API** credential, choose **Sandbox (Synthetic Data)**, enter your sandbox key and test the credential.
4. Select that credential on both **Screen fictional subject** and **Retrieve synthetic evidence**.
5. Start **Run synthetic example** manually. The first SanctionsKit node returns the complete screening result. The second uses its `id` to retrieve evidence for that same result.
6. Inspect the returned environment, coverage, status and interpretation disclaimer.

No API key, saved credential reference, pinned output or customer data is included. Keep credentials and response exports out of shared templates.

## Behavior and limits

The idempotency key uses the n8n execution ID and item index so independent demonstration runs are distinct. For a real screening event, choose a stable event identifier and reuse the same key with unchanged input when retrying that event.

Errors stop this starter by default. It has no automatic retry loop, schedule, external notification, approval decision or human-review branch. Add those deliberately when adapting a workflow.

A potential match requires review. A no-match applies only to the selected coverage and policy; it does not establish identity or legal clearance. The synthetic example does not measure production screening quality.

## Source and license

Maintained by SanctionsKit. The workflow is based on `SanctionsKit/n8n-nodes-sanctionskit`, tag `v0.1.0`, file `examples/synthetic-screening.json`. These setup notes were added for this contribution.

This file is a sanitized n8n `2.41.4` export, imported and re-exported in isolated local state. The original nodes, parameters, expressions, connections and settings are unchanged. Generated timestamps, version metadata and sharing metadata were removed; the original authored workflow ID is retained. No credentials were configured and no authenticated workflow execution was performed for this contribution.

The template retains its original MIT license and copyright notice in [the accompanying license](SanctionsKit%20synthetic%20screening%20and%20evidence.LICENSE.txt).
