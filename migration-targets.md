# Production migration targets

This list records the planned scope and current enrollment. The server and actions client passed protected merge verification. The actions client also published and verified its first JS-action release. The dotfiles client also passed its protected merge. The dbt-authorized-models client also passed its protected merge and immutable source-only release verification. The list comes from securefix-server's policy.json, excluding the retired securefix-server family and adding csm-ci-actions and csm-ci-server.

| Repository | Release requirement | Enrollment |
| --- | --- | --- |
| civitaspo/csm-ci-server | No release pipeline required | Protected merge verified in PR #5 |
| civitaspo/csm-ci-actions | JavaScript action; verify committed dist | Protected merge and immutable v0.1.0 release verified; see verification.md |
| civitaspo/dotfiles | No release pipeline in the old policy | Protected merge verified in PR #266; no release pipeline required |
| civitaspo/dbt-authorized-models | GitHub release; verify source-only requirements | Protected merge PR #172 and immutable v0.4.1 release verified; see verification.md |
| civitaspo/dbt-snowflake-iceberg-sync | GitHub release; verify source-only requirements | Caller bootstrap PR #135 merged; protection and release verification pending |
| civitaspo/dbt-snowflake-rap-enforcement | GitHub release; verify source-only requirements | Pending caller and release migration |
| civitaspo/terraform-provider-sigma | Terraform Registry manifest, checksums and GPG signatures | Blocked on a compatible publishing design; do not replace with ordinary GoReleaser publishing |
| civitaspo/herdr-linear-agent | Rust binaries; preserve the existing asset and target contract | Pending caller and release migration |
| civitaspo/hook-relay-client-rs | Rust binaries; preserve the existing asset and target contract | Pending caller and release migration |
| civitaspo/herdr-infobox | Rust binaries; preserve the existing asset and target contract | Pending caller and release migration |

Do not enroll a client merely because its App installation or credentials are ready. First verify its new callers, required checks, legacy ruleset transition, and release requirements. Keep the old service in place until the last client has migrated and been verified. Do not grant the retired server bot trust in the new configuration.
