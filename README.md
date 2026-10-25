# csm-ci-server

GitHub Actions server for validated CI requests from civitaspo repositories, using [csm-ci-actions](https://github.com/civitaspo/csm-ci-actions) pinned to `85d8fc79ef0ca89ec4cc073d142dab75ebe09a66`.

## Enrollment and protection

Only this server is enrolled in `csm-ci.yaml`. The label caller receives requests, the client callers request approval, policy checks and merges for this repository, and `csm-ci-check` validates configuration and workflows. Its `status-check` job is the required CI check.

Repository-settings reconciliation selects only this server. The scheduled workflow repairs settings and rulesets, preserves an emergency stop on `controlled-merges`, and opens managed-file PRs. Manual dispatch defaults to `managed-files-only`; select `reconcile` only after verifying the default-branch callers and required checks.

Production App approval and merge were verified in [Renovate PR #2](https://github.com/civitaspo/csm-ci-server/pull/2). Review the [verification record](verification.md) before activating protection. To bootstrap the managed PR template, dispatch `csm-ci-repo-settings` with `repository: civitaspo/csm-ci-server` and `mode: managed-files-only`, review and merge the generated PR, then dispatch `reconcile`. Confirm all four managed rulesets and run a separate intended PR through `/approve` and `/merge` under protection before enrolling a client.

## App installations and credentials

Install `csm-ci-client` on this server only. Install `csm-ci-server` and `csm-ci-approver` on the server and the clients listed in [migration-targets.md](migration-targets.md).

Server and Approver private keys belong only in this repository's `csm-ci` environment, limited to `main`. Their variables are `CSM_CI_SERVER_APP_CLIENT_ID` and `CSM_CI_APPROVER_APP_CLIENT_ID`; their secrets are `CSM_CI_SERVER_APP_PRIVATE_KEY` and `CSM_CI_APPROVER_APP_PRIVATE_KEY`.

Every client, including this server, needs repository variables `CSM_CI_CLIENT_APP_CLIENT_ID` and `CSM_CI_SERVER_REPOSITORY`, plus the `CSM_CI_CLIENT_APP_PRIVATE_KEY` secret. The server repository variable is `civitaspo/csm-ci-server`.

## Operating changes

Use pull requests for every configuration or workflow change. The server treats every path here as sensitive. A trusted operator uses `/approve`, then `/merge` after policy and CI checks pass. The trusted identities were ported from the previous policy by immutable user ID, excluding the retired server bot. The Renovate sensitive-path exemption does not apply to this server or csm-ci-actions.

Enroll clients individually after reviewing their caller and required-check migration. Follow the [setup guide](https://github.com/civitaspo/csm-ci-actions/blob/85d8fc79ef0ca89ec4cc073d142dab75ebe09a66/docs/setup.md) and [repository settings guide](https://github.com/civitaspo/csm-ci-actions/blob/85d8fc79ef0ca89ec4cc073d142dab75ebe09a66/docs/repo-settings.md).
