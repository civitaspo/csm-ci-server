# Production migration targets

This list records the planned scope and current enrollment. The server and actions client passed protected merge verification. The actions client also published and verified its first JS-action release. The dotfiles client also passed its protected merge. The dbt-authorized-models and dbt-snowflake-iceberg-sync clients also passed their protected merges and immutable source-only release verification. The list comes from securefix-server's policy.json, excluding the retired securefix-server family and adding csm-ci-actions and csm-ci-server.

| Repository | Release requirement | Enrollment |
| --- | --- | --- |
| civitaspo/csm-ci-server | No release pipeline required | Protected merge verified in PR #5 |
| civitaspo/csm-ci-actions | JavaScript action; verify committed dist | Protected merge and immutable v0.1.0 release verified; see verification.md |
| civitaspo/dotfiles | No release pipeline in the old policy | Protected merge verified in PR #266; no release pipeline required |
| civitaspo/dbt-authorized-models | GitHub release; verify source-only requirements | Protected merge PR #172 and immutable v0.4.1 release verified; see verification.md |
| civitaspo/dbt-snowflake-iceberg-sync | GitHub release; verify source-only requirements | Protected merge PR #136 and immutable v0.6.4 release verified; see verification.md |
| civitaspo/dbt-snowflake-rap-enforcement | Source-only immutable GitHub release | Complete: protected release PR #173 and verified v0.5.5 publication |
| civitaspo/terraform-provider-sigma | Terraform Registry manifest, checksums and GPG signatures | Caller PR #214 and full main CI verified; protection, protected production release and Registry/Terraform CLI checks remain required |
| civitaspo/herdr-linear-agent | Four native Darwin/musl raw binaries, sidecar checksums and provenance; fixed manifest versions | Complete: protected release PR #201, immutable v0.8.2, four native binaries and actual installer verified |
| civitaspo/hook-relay-client-rs | Four raw hook-relay-client Darwin/musl binaries, checksum sidecars and provenance; fixed Cargo version | Complete: protected release PR #68, immutable v0.1.1, four native binaries and README installation verified |
| civitaspo/herdr-infobox | Source-only GitHub release; local binary packaging remains deferred | Complete: protected release PR #95 and verified immutable source-only v0.1.1 publication |

Do not enroll a client merely because its App installation or credentials are ready. First verify its new callers, required checks, legacy ruleset transition, and release requirements. Keep the old service in place until the last client has migrated and been verified. Do not grant the retired server bot trust in the new configuration.
