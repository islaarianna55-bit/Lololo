# Guard row keys and conflicting duplicates before Google Sheets import

This original AI-assisted synthetic example validates a whole batch before returning items to a downstream writer. It collapses identical composite string keys and stops on conflicting payloads, missing/blank/numeric/padded keys, or batches over 10,000 items. The batch cap is a guard, not a performance guarantee.

Import the JSON, inspect the Code nodes, and run the manual trigger. Three synthetic rows should become two output items. Set `DEMONSTRATE_CONFLICT` to `true` in **Synthetic rows** to demonstrate a blocked conflict. No credentials, external network, customer data or paid modules are needed.

Replace **Synthetic rows** with parsed input only after testing. Use **Run Once for All Items** and **Stop Workflow**; leave **Always Output Data** off. The example has no Google Sheets writer. A later append-or-update integration needs explicit key mapping, text IDs and a safe test destination. This is not a transaction, concurrency lock or cross-run deduplication store.

The workflow was exported by the real n8n 2.43.8 CLI from a fresh isolated container. Top-level instance/owner/version IDs and timestamps were removed for portability; description and this documentation note were added. Code-node logic and connections are unchanged. The imported workflow completed with two items, composite keys for `catalog-a/001` and `catalog-b/001`, and paired input indices 0 and 2. Source checks and prior CLI fixtures also cover conflict and blank-key failures. Live Sheets writes, n8n Cloud, browser import and concurrent execution are unverified.

Source and full verification scope: https://github.com/brodyga45/n8n-sheet-import-guard-20261019

Commercial disclosure: the author is evaluating n8n's affiliate programme and has no approved referral link in this contribution. The collection maintainer's existing referral links belong to that maintainer. A contribution, download or successful synthetic run is not evidence of a paying customer or commission. Existing users may use their current workspace; a one-off import may suit a local script instead.

## License for this template and these notes

MIT License

Copyright (c) 2026 brodyga45

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
