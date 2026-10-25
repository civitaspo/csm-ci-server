# csm-ci-server

GitHub Actions server for validated CI requests from civitaspo repositories, using [csm-ci-actions](https://github.com/civitaspo/csm-ci-actions) pinned to `313dac1ce1e5a288fc2f5fe729e8f4ca7ad71180`.

## Enrollment and protection

The configured repositories and enabled operations are listed in `csm-ci.yaml`; [migration-targets.md](migration-targets.md) records each client's verification state. The label caller handles requests, and `csm-ci-check` validates configuration and workflows. Its `status-check` job is the required CI check.

Repository-settings reconciliation selects configured, installed clients and then this server. Scheduled runs repair settings and rulesets, preserve an emergency stop on `controlled-merges`, and open managed-file PRs. Manual dispatch defaults to `managed-files-only`. For a new client, merge and verify its callers first, then dispatch `reconcile` for that repository and verify the protections before retiring saved legacy service rulesets.

The server passed its protected merge in PR #5. Read [verification.md](verification.md) for production approvals, merges and release evidence. Client publication requires the bounded `release-pr` route and its declared build type. The trusted server workflows use the runtime revision above. Managed approve, merge and policy-check callers use separately reviewed revision `2b751e0f2e16bb8e7bdd57994a8d3e13cdb1879e`, whose defining-repository lock covers their composite dependencies for clients that require action lockfiles.

## App installations and credentials

Install `csm-ci-client` on this server only. Install `csm-ci-server` and `csm-ci-approver` on the server and the clients listed in [migration-targets.md](migration-targets.md).

Server and Approver private keys belong only in this repository's `csm-ci` environment, limited to `main`. Their variables are `CSM_CI_SERVER_APP_CLIENT_ID` and `CSM_CI_APPROVER_APP_CLIENT_ID`; their secrets are `CSM_CI_SERVER_APP_PRIVATE_KEY` and `CSM_CI_APPROVER_APP_PRIVATE_KEY`.

Every client, including this server, needs repository variables `CSM_CI_CLIENT_APP_CLIENT_ID` and `CSM_CI_SERVER_REPOSITORY`, plus the `CSM_CI_CLIENT_APP_PRIVATE_KEY` secret. The server repository variable is `civitaspo/csm-ci-server`.

## Operating changes

Use pull requests for every configuration or workflow change. The server treats every path here as sensitive. A trusted operator uses `/approve`, then `/merge` after policy and CI checks pass. The trusted identities were ported from the previous policy by immutable user ID, excluding the retired server bot. The Renovate sensitive-path exemption does not apply to this server or csm-ci-actions.

Enroll clients individually after reviewing their caller and required-check migration. Follow the [setup guide](https://github.com/civitaspo/csm-ci-actions/blob/313dac1ce1e5a288fc2f5fe729e8f4ca7ad71180/docs/setup.md) and [repository settings guide](https://github.com/civitaspo/csm-ci-actions/blob/313dac1ce1e5a288fc2f5fe729e8f4ca7ad71180/docs/repo-settings.md).
