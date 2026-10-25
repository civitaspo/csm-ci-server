# Production migration targets

This list records the planned scope and current enrollment. The server passed protected merge verification; the actions client is selected for bootstrap but its callers and protection still require verification. The list comes from securefix-server's policy.json, excluding the retired securefix-server family and adding csm-ci-actions and csm-ci-server.

| Repository | Release requirement | Enrollment |
| --- | --- | --- |
| civitaspo/csm-ci-server | No release pipeline required | Protected merge verified in PR #5 |
| civitaspo/csm-ci-actions | JavaScript action; verify committed dist | Selected for managed caller bootstrap; JS release callers and protection verification pending |
| civitaspo/dotfiles | No release pipeline in the old policy | Pending caller migration |
| civitaspo/dbt-authorized-models | GitHub release; verify source-only requirements | Pending caller and release migration |
| civitaspo/dbt-snowflake-iceberg-sync | GitHub release; verify source-only requirements | Pending caller and release migration |
| civitaspo/dbt-snowflake-rap-enforcement | GitHub release; verify source-only requirements | Pending caller and release migration |
| civitaspo/terraform-provider-sigma | Terraform Registry manifest, checksums and GPG signatures | Blocked on a compatible publishing design; do not replace with ordinary GoReleaser publishing |
| civitaspo/herdr-linear-agent | Rust binaries; preserve the existing asset and target contract | Pending caller and release migration |
| civitaspo/hook-relay-client-rs | Rust binaries; preserve the existing asset and target contract | Pending caller and release migration |
| civitaspo/herdr-infobox | Rust binaries; preserve the existing asset and target contract | Pending caller and release migration |

Do not enroll a client merely because its App installation or credentials are ready. First verify its new callers, required checks, legacy ruleset transition, and release requirements. Keep the old service in place until the last client has migrated and been verified. Do not grant the retired server bot trust in the new configuration.
