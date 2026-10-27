# csm-ci-server

This repository runs [csm-ci-actions](https://github.com/civitaspo/csm-ci-actions) in server mode for civitaspo repositories. It receives validated CI requests from client repositories and performs approvals, policy checks, merges, releases and repository-settings reconciliation on their behalf.

[`csm-ci.yaml`](csm-ci.yaml) lists the enrolled repositories, trusted identities and enabled operations. The workflows in [`.github/workflows`](.github/workflows) call the reusable csm-ci-actions workflows at pinned revisions.

## Setup

Follow the csm-ci-actions [setup guide](https://github.com/civitaspo/csm-ci-actions/blob/main/docs/setup.md) and [repository settings guide](https://github.com/civitaspo/csm-ci-actions/blob/main/docs/repo-settings.md).

- Install `csm-ci-client` on this server only. Install `csm-ci-server` and `csm-ci-approver` on this server and every client in `csm-ci.yaml`.
- The `csm-ci` environment, limited to `main`, holds the Server and Approver App credentials: variables `CSM_CI_SERVER_APP_CLIENT_ID` and `CSM_CI_APPROVER_APP_CLIENT_ID`, secrets `CSM_CI_SERVER_APP_PRIVATE_KEY` and `CSM_CI_APPROVER_APP_PRIVATE_KEY`.
- The same environment holds the Terraform provider signing secrets `CSM_CI_TERRAFORM_PROVIDER_GPG_PRIVATE_KEY` and `CSM_CI_TERRAFORM_PROVIDER_GPG_PASSPHRASE`, and the variable `CSM_CI_TERRAFORM_PROVIDER_GPG_FINGERPRINT`.
- Every client, including this server, needs repository variables `CSM_CI_CLIENT_APP_CLIENT_ID` and `CSM_CI_SERVER_REPOSITORY` (`civitaspo/csm-ci-server`), and the secret `CSM_CI_CLIENT_APP_PRIVATE_KEY`.

To enroll a client, add it to `csm-ci.yaml`, merge and verify its caller workflows, then dispatch `csm-ci-repo-settings` for that repository and verify its rulesets.

## Changes

Change this repository through pull requests. Every path is sensitive, so a trusted operator comments `/approve`, then `/merge` after `csm-ci/policy` and `status-check` pass. Bot mergers are not allowed here.
