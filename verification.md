# Production verification

## 2026-10-25: first App approval and merge

[Renovate PR #2](https://github.com/civitaspo/csm-ci-server/pull/2) changed only `renovate.json` at signed head `80c59d74b7be68b3f37ccfb5bd5eb71e926957b5`.

- [Configuration and workflow CI](https://github.com/civitaspo/csm-ci-server/actions/runs/40200040619) passed, including `status-check`.
- The initial automatic approval made no review. The Server App's `csm-ci/policy` check failed because every server path requires explicit approval.
- The owner posted [`/approve`](https://github.com/civitaspo/csm-ci-server/pull/2#issuecomment-6398562732). [The server run](https://github.com/civitaspo/csm-ci-server/actions/runs/40200120806) passed.
- Review `5643566812` was `APPROVED` by `csm-ci-approver[bot]` (user ID `347823687`) on the exact head. Its body recorded `<!-- csm-ci:approve:command:6398562732 -->`. The policy check owned by Server App `5467106` then passed.
- The owner posted [`/merge`](https://github.com/civitaspo/csm-ci-server/pull/2#issuecomment-6398576836). [The server run](https://github.com/civitaspo/csm-ci-server/actions/runs/40200231900) passed.
- The PR was squash-merged as `d2a2cf619d04fa201de51abee18d71e5fb5b16e5` by `csm-ci-server[bot]` (user ID `347821498`).

Repository rulesets were not yet active. This proves the production request, approval, policy and merge path. A separate intended PR must prove the same path under the managed rulesets after activation; the initial run alone does not prove that GitHub counted the App review toward required approval.

## Remaining migration gates

Enable repository-settings reconciliation for the server, distribute and review its managed PR template, activate the four managed rulesets, and verify a protected merge. Enroll each client only after its callers and required checks are verified. Preserve the existing release asset and signing contracts before retiring any old release pipeline. Archive the old repositories and remove the old bot credentials only after the final client no longer depends on them. Credential removal is performed by the owner.
