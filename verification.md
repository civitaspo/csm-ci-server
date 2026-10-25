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

## 2026-10-25: managed files and protection activation

[Settings PR #3](https://github.com/civitaspo/csm-ci-server/pull/3) was merged by the Server App as `d5f5f5e005e84273b4a99d53e04cd6f7ca64fd1f`; [main CI](https://github.com/civitaspo/csm-ci-server/actions/runs/40201465177) passed.

The [managed-files-only dispatch](https://github.com/civitaspo/csm-ci-server/actions/runs/40201510211) passed and opened [PR #4](https://github.com/civitaspo/csm-ci-server/pull/4) containing only the configured PR template. API inspection still showed no rulesets and unchanged merge-method settings. Its signed Server App commit received an exact-head Approver App review after `/approve`, passed policy and CI, and was merged by the Server App as `91f1ec9dca3f0370ef72fe4d81dae951d712dfed`. [Main CI](https://github.com/civitaspo/csm-ci-server/actions/runs/40201734041) passed.

The [reconcile dispatch](https://github.com/civitaspo/csm-ci-server/actions/runs/40201817007) passed. API inspection confirmed four active rulesets: integrity `25823865`, review-and-checks `25823866`, controlled-merges `25823867`, and release-tags `25823869`. Integrity and review-and-checks have no bypass. The required approval count is one with stale review dismissal. Required checks are `status-check` from App `15368` and `csm-ci/policy` from App `5467106`. Controlled merges allow only Server App `5467106` with a pull-request bypass; release tags allow only that App. Squash-only merging, branch cleanup and immutable releases are enabled.

The next intended PR updates all actions pins to `85d8fc79ef0ca89ec4cc073d142dab75ebe09a66`. That revision passed [main CI](https://github.com/civitaspo/csm-ci-actions/actions/runs/40201184382) and [post-merge smoke](https://github.com/civitaspo/csm-ci-actions/actions/runs/40201237956). Verify this PR through `/approve` and `/merge` under active protection before enrolling the actions repository.

## 2026-10-25: protected server merge

[PR #5](https://github.com/civitaspo/csm-ci-server/pull/5) updated the reviewed workflow pins at signed head `5908bb8c31e4fdf461ddfa08b2f225d400a6ff2e`. Before approval, GitHub reported `REVIEW_REQUIRED` and `BLOCKED`; [CI](https://github.com/civitaspo/csm-ci-server/actions/runs/40202140663) passed and policy failed as expected for sensitive server paths.

The owner requested [`/approve`](https://github.com/civitaspo/csm-ci-server/pull/5#issuecomment-6398857991). Review `5643661950` was `APPROVED` by App bot `347823687` on that exact head, with its command marker. GitHub's review decision became `APPROVED`, and Server App `5467106`'s policy check passed. Integrity and review/check rulesets still had no bypass.

The owner requested [`/merge`](https://github.com/civitaspo/csm-ci-server/pull/5#issuecomment-6398868380). The [server merge run](https://github.com/civitaspo/csm-ci-server/actions/runs/40202334965) passed and merged the PR as `b54398af51fbfea8aec3dd8849200b7fdbc8a4a9`, with `merged_by` equal to Server bot `347821498`. [Main CI](https://github.com/civitaspo/csm-ci-server/actions/runs/40202378983) passed. This verifies that the Approver App review counts under the managed required-approval ruleset and the Server App can merge through controlled protection.

## Actions client bootstrap

The configuration now selects `civitaspo/csm-ci-actions` and enables its existing JS-action release type and bounded release-PR route. Three common approval, policy and merge caller files are managed globally at the reviewed revision. Bootstrap them with `managed-files-only` and merge through the client's current process before activation. Release callers and the root package version field must be added and verified before reconciling protection; the current actions package has no version field. Do not treat enrollment alone as completed migration.

## 2026-10-25: actions client migration and first release

[Caller PR #37](https://github.com/civitaspo/csm-ci-actions/pull/37) passed CI and received an exact-head Approver App review before the Server App merged it. [Main CI](https://github.com/civitaspo/csm-ci-actions/actions/runs/40206032238) and [release preparation](https://github.com/civitaspo/csm-ci-actions/actions/runs/40206032304) passed.

The [actions reconcile run](https://github.com/civitaspo/csm-ci-server/actions/runs/40206198160) activated four rulesets: integrity `25825270`, review-and-checks `25825271`, controlled-merges `25825272`, and release-tags `25825273`. Integrity and review-and-checks have no bypass; required checks bind `status-check` to App `15368` and `csm-ci/policy` to App `5467106`. The only controlled-merge and release-tag bypass is Server App `5467106`. Squash-only merging, branch cleanup and immutable releases are enabled.

Under those protections, [release PR #38](https://github.com/civitaspo/csm-ci-actions/pull/38) changed from `REVIEW_REQUIRED` to `APPROVED` after review `5643840344` on exact head `2ece518e58df8ff7be843363e7fdccd135688eb6`. Policy and CI passed. The Server App merged it as `bd2f3d06cf2f411af88eda876962585aadda0d2e`; [main CI](https://github.com/civitaspo/csm-ci-actions/actions/runs/40206695363) passed.

The [production release pipeline](https://github.com/civitaspo/csm-ci-server/actions/runs/40206708445) passed intake, planning, JS-action build, publication and cleanup. The builder installed frozen dependencies, rebuilt the action and verified committed `dist/`. API inspection confirmed that [v0.1.0](https://github.com/civitaspo/csm-ci-actions/releases/tag/v0.1.0) is published, immutable and authored by the Server App. Its annotated tag points to the merge SHA. Its exact asset set is `SHA256SUMS`, matching the JS-action contract; its SHA256 is `01ba4719c80b6fe911b091a7c05124b64eeece964e09c058ef8f9805daca546b`.

Local `gh attestation verify` succeeded with the reusable signer workflow `civitaspo/csm-ci-actions/.github/workflows/server.yml`, the release predicate type and self-hosted runners denied. The verified statement matched the asset digest, client `civitaspo/csm-ci-actions`, merge SHA and tag `v0.1.0`. An earlier attempt incorrectly selected the calling server repository as signer and failed; correcting the expected reusable signer resolved it.

The release-commit push prepared duplicate PR #39 before tag publication. Its only change was one changelog line for the same version. After verifying publication, the operator closed it and deleted only its unchanged Server App branch. No second release was published.

Caller-only migrations reuse completed Phase 6 implementation verification. Broader suites are required only for implementation changes, failures or specific unresolved concerns; each target still requires actual production approval, protected merge and its own release contract verification.

## Dotfiles migration gates

Only dotfiles is newly selected. Its initial snapshot has legacy `controlled-merges` (old Server App only), `review-and-checks` (old policy context and bypass), and `default-branch` rulesets. Bootstrap the common callers without changing those protections, merge via the existing service, then reconcile the new rulesets. Verify the new required checks and App approval with a protected PR that removes the retired callers. Dotfiles has no release pipeline in the old policy. Other targets remain pending.
