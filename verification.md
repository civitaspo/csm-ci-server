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

## 2026-10-25: dotfiles protected migration completed

The old service merged [bootstrap PR #263](https://github.com/civitaspo/dotfiles/pull/263) before protection changed. The [reconcile dispatch](https://github.com/civitaspo/csm-ci-server/actions/runs/40208743048) passed. API inspection confirmed four active managed rulesets (integrity `25826305`, review-and-checks `25826306`, controlled-merges `25826307`, release-tags `25826308`), exact required check App IDs, one stale-dismissed approval, and only the Server App's controlled-merge and release-tag bypasses. Squash-only merges, branch cleanup and immutable releases are enabled. The unrelated default-branch ruleset remains unchanged. The two legacy rulesets were removed only after the new protection was verified.

[PR #266](https://github.com/civitaspo/dotfiles/pull/266) updated the operator guide and corrected a CI header comment. Its initial policy failed and GitHub required review. After the owner requested [`/approve`](https://github.com/civitaspo/dotfiles/pull/266#issuecomment-6399801356), Approver App review `5644006525` approved exact head `861dd7b17fd6f51bd4861634d6994dabdb8c3bc3` with the command marker. Required checks passed, including policy from App `5467106`; GitHub counted the review without a review-ruleset bypass. After [`/merge`](https://github.com/civitaspo/dotfiles/pull/266#issuecomment-6399810838), the new Server App merged `72889230e7fb67791b102c8413a22326f25f9f98`. [Main CI](https://github.com/civitaspo/dotfiles/actions/runs/40209284971) passed. Existing PRs were refreshed where they lacked the new policy; unrelated sensitive PRs still require owner approval. Dotfiles had no release pipeline in the previous policy, so its release gate does not apply.

## dbt-authorized-models enrollment gates

[Caller bootstrap PR #166](https://github.com/civitaspo/dbt-authorized-models/pull/166) replaces the old callers and embedded workflow autofix request, preserving dbt Core and Fusion CI. Release preparation pins actions revision `e06b90a7e1b21b12549b5b2b11b584de410f1d6a`, whose [main CI](https://github.com/civitaspo/csm-ci-actions/actions/runs/40210771217) passed. Its implementation passed the four release-PR scenarios recorded in the actions handoff, including dbt/Python version synchronization and repeated preparation without duplicate changes. Other callers retain the previously verified revision.

The selected client uses build type `none`: its previous release contract had no binary assets. The global bounded release route adds only root `dbt_project.yml: version-only`. Workflow autofix is enabled explicitly; the same signed-commit, trusted-committer, artifact/run validation and sensitive-path approval rules still apply. No credential or App permission changes are included. The old client's release callers are disabled and its unchanged legacy release PR/branch retired; the old settings and caller distributors remain disabled throughout migration.

After bootstrap merges, reconcile the four managed rulesets, verify exact bypasses and check producers, and remove only the two saved legacy rulesets. Keep its unrelated default-branch and all-tags rulesets. Then verify a real protected merge and source-only immutable release with synchronized version files, exact tag/merge binding, `SHA256SUMS`, and locally verified provenance before selecting another client.

The original approval attempt for bootstrap PR #166 was rejected by the old server [run](https://github.com/civitaspo/securefix-server/actions/runs/40211655859): its caller references `bf66565ea834e0fe528f4655806335740afd3348`, but the active runtime requires `1b35591e74eeae8fe848d16c6fcae0ba8fb75f6c`. No approval or merge bypass was requested from that service. The owner-authorized recovery waited for the [original full PR CI run](https://github.com/civitaspo/dbt-authorized-models/actions/runs/40211357216) to pass all package and Fusion jobs. It kept the PR and `status-check` requirements active, temporarily removed only the unavailable legacy review/policy gate and merge restriction, and merged exact head `bdd95436097da93426e90615027f16c81bf38d5b` as `28e7f017780f8ea249a3872403321d0d9b5b3c81`. Both saved legacy rulesets were restored immediately afterward. The PR workflow intentionally skips its failure-only `status-check` when all jobs succeed; the original full run was checked separately. Main-branch CI is still pending. This exception does not count as the new system's protected-merge verification.
