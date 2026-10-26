# Production migration targets

All ten migration targets completed protected merge verification and their applicable publication, provenance, and installation checks. The table records each completed enrollment. The list comes from securefix-server's policy.json, excluding the retired securefix-server family and adding csm-ci-actions and csm-ci-server.

| Repository | Release requirement | Enrollment |
| --- | --- | --- |
| civitaspo/csm-ci-server | No release pipeline required | Protected merge verified in PR #5 |
| civitaspo/csm-ci-actions | JavaScript action; verify committed dist | Protected merge and immutable v0.1.0 release verified; see verification.md |
| civitaspo/dotfiles | No release pipeline in the old policy | Protected merge verified in PR #266; no release pipeline required |
| civitaspo/dbt-authorized-models | GitHub release; verify source-only requirements | Protected merge PR #172 and immutable v0.4.1 release verified; see verification.md |
| civitaspo/dbt-snowflake-iceberg-sync | GitHub release; verify source-only requirements | Protected merge PR #136 and immutable v0.6.4 release verified; see verification.md |
| civitaspo/dbt-snowflake-rap-enforcement | Source-only immutable GitHub release | Complete: protected release PR #173 and verified v0.5.5 publication |
| civitaspo/terraform-provider-sigma | Terraform Registry manifest, checksums and GPG signatures | Complete: protected release PR #215, immutable v0.2.4, thirteen platforms, GPG signature, Registry discovery and actual Terraform installation/schema verified |
| civitaspo/herdr-linear-agent | Four native Darwin/musl raw binaries, sidecar checksums and provenance; fixed manifest versions | Complete: protected release PR #201, immutable v0.8.2, four native binaries and actual installer verified |
| civitaspo/hook-relay-client-rs | Four raw hook-relay-client Darwin/musl binaries, checksum sidecars and provenance; fixed Cargo version | Complete: protected release PR #68, immutable v0.1.1, four native binaries and README installation verified |
| civitaspo/herdr-infobox | Source-only GitHub release; local binary packaging remains deferred | Complete: protected release PR #95 and verified immutable source-only v0.1.1 publication |

Every enrolled client passed caller, required-check, legacy ruleset transition and applicable release verification before the old services were archived. App installation and credential readiness alone did not satisfy enrollment. The retired server bot has no trust in the new configuration.
