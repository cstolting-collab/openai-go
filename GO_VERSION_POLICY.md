# Go Version Policy

The OpenAI Go SDK normally supports the current stable Go release and the
immediately preceding stable Go release when its secure dependency graph allows
both lines. The oldest supported release is declared by the `go` directive in
[`go.mod`](go.mod) and is tested on every pull request. A security or dependency
requirement may force an earlier minimum increase, subject to SDK CODEOWNERS
approval and the release requirements below.

The SDK team may retain the most recently retired Go release for up to six
months when the dependency graph and security posture allow it. This grace
period is discretionary, is not an LTS commitment, and may end early because of
security, dependency, platform, or toolchain requirements. During a grace
period, CI tests the retired minimum in addition to the current and preceding
stable Go releases.

Minimum Go version increases:

- ship in an SDK minor release, not a patch release;
- are documented in the README and release notes;
- require approval from the SDK CODEOWNERS; and
- do not require a new SDK major version when exported APIs and the module
  import path remain compatible.

The SDK team reviews this policy within 30 days of each scheduled February and
August Go release. Each month, a scheduled Codex workflow reads a snapshot of
the official Go release feed and the repository policy. If the repository has
drifted and no generated update is already open, it opens a draft pull request
containing the proposed module, CI, documentation, and release-note changes.
The workflow never overwrites an existing draft or merges a proposal
automatically; the normal compatibility checks and SDK CODEOWNER review decide
whether it ships.

An active grace period must be recorded in the current-compatibility section
with an explicit end date and reason. In the absence of that record, automation
proposes the current and immediately preceding stable Go releases.

### Current compatibility

| SDK version | Go requirement |
| --- | --- |
| v3.45.0 through v3.64.3 | Go 1.25 or later |
| Next minor release (proposal pending CODEOWNERS approval) | Go 1.26 or later |
| v3.44.0 | Final release that builds with Go 1.22–1.24 |

The proposed early end of Go 1.25 support is dated 2026-10-09. The scheduled
`govulncheck` scan found five reachable advisories in `golang.org/x/net v0.58.0`;
its reported fixed version, v0.60.0, declares Go 1.26.0. The required Go 1.25
CI run confirms that the fixed module cannot be used with Go 1.25. The proposed
minimum increase must ship in a minor release and receive SDK CODEOWNERS approval.
If approval is not granted, maintainers must choose another security disposition;
this proposal must not be shipped as a patch release.

Previously published SDK versions remain available. Unsupported Go releases
and older SDK versions receive no guaranteed fixes or security backports. Users
who need current security fixes must use a supported Go toolchain and SDK
release.

For the upstream lifecycle and toolchain-selection rules, see the [Go release
policy](https://go.dev/doc/devel/release#policy) and [Go toolchain
documentation](https://go.dev/doc/toolchain).
