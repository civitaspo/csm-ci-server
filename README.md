# csm-ci-server

GitHub Actions server for validated CI requests from civitaspo repositories, using [csm-ci-actions](https://github.com/civitaspo/csm-ci-actions) pinned to `82917b4c3a28e5ff4023991ed355b40081d650fb`.

## Enrollment and protection

The configured repositories and enabled operations are listed in `csm-ci.yaml`; [migration-targets.md](migration-targets.md) records each client's verification state. The label caller handles requests, and `csm-ci-check` validates configuration and workflows. Its `status-check` job is the required CI check.

Repository-settings reconciliation selects configured, installed clients and then this server. Scheduled runs repair settings and rulesets, preserve an emergency stop on `controlled-merges`, and open managed-file PRs. Manual dispatch defaults to `managed-files-only`. For a new client, merge and verify its callers first, then dispatch `reconcile` for that repository and verify the protections before retiring saved legacy service rulesets.

The server passed its protected merge in PR #5. Read [verification.md](verification.md) for production approvals, merges and release evidence. Client publication requires the bounded `release-pr` route and its declared build type. The trusted server workflows use the runtime revision above. Managed approve and policy-check callers retain reviewed revision `2b751e0f2e16bb8e7bdd57994a8d3e13cdb1879e`. Managed merge callers use `5e26d290f13f8db3d067aabc4d14030f95c40b8a`, which forwards native Bot comments on their own PRs. Both revisions include defining-repository locks for clients that require action lockfiles.

## App installations and credentials

Install `csm-ci-client` on this server only. Install `csm-ci-server` and `csm-ci-approver` on the server and the clients listed in [migration-targets.md](migration-targets.md).

Server and Approver private keys belong only in this repository's `csm-ci` environment, limited to `main`. Their variables are `CSM_CI_SERVER_APP_CLIENT_ID` and `CSM_CI_APPROVER_APP_CLIENT_ID`; their secrets are `CSM_CI_SERVER_APP_PRIVATE_KEY` and `CSM_CI_APPROVER_APP_PRIVATE_KEY`.

The provider publisher also uses the human-provisioned `CSM_CI_TERRAFORM_PROVIDER_GPG_PRIVATE_KEY` and `CSM_CI_TERRAFORM_PROVIDER_GPG_PASSPHRASE` secrets in the same environment. Its public `CSM_CI_TERRAFORM_PROVIDER_GPG_FINGERPRINT` variable is the complete primary fingerprint `12D7B26BEB5394D7AA579FB300F0D373EFFA1DC6`, matching the existing Terraform Registry registration. Build jobs and other release types receive no signing credentials.

Every client, including this server, needs repository variables `CSM_CI_CLIENT_APP_CLIENT_ID` and `CSM_CI_SERVER_REPOSITORY`, plus the `CSM_CI_CLIENT_APP_PRIVATE_KEY` secret. The server repository variable is `civitaspo/csm-ci-server`.

## Operating changes

Use pull requests for every configuration or workflow change. The server treats every path here as sensitive. A trusted operator uses `/approve`, then `/merge` after policy and CI checks pass. The trusted identities were ported from the previous policy by immutable user ID, excluding the retired server bot. Renovate is the only configured Bot merger. On eligible clients, it can request `/merge` only for its own PR containing only its own commits, after current approval, policy and required checks pass. Bot mergers and the Renovate sensitive-path exemption remain forbidden on this server and csm-ci-actions, so both repositories require a trusted operator. Update client merge callers sequentially before treating Renovate auto-merges as enabled. This server configures Renovate non-major updates to request `/merge` through PR comments, with platform automerge disabled. On this server and csm-ci-actions those requests remain subject to the fixed Bot-merger prohibition and require operator approval and a new operator `/merge` request.

Enroll clients individually after reviewing their caller and required-check migration. Follow the [setup guide](https://github.com/civitaspo/csm-ci-actions/blob/82917b4c3a28e5ff4023991ed355b40081d650fb/docs/setup.md) and [repository settings guide](https://github.com/civitaspo/csm-ci-actions/blob/82917b4c3a28e5ff4023991ed355b40081d650fb/docs/repo-settings.md).
