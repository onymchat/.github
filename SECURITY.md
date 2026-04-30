# Security Policy

Onym is a privacy-critical messaging system. Group membership confidentiality relies on the soundness of the Groth16 circuits, the correctness of the Soroban verifier, and the key handling inside the Swift / Kotlin SDKs and the iOS / Android apps. Security issues in any of these components can deanonymize members, forge membership, or break epoch monotonicity. We take reports seriously and ask that they be disclosed privately.

This policy applies to all repositories under the [`onymchat`](https://github.com/onymchat) organization. Per-repo `SECURITY.md` files override this default when present.

## Supported Versions

Only the latest minor release line of each component receives security updates. Older tags are archived and will not be patched.

| Version | Supported |
| ------- | --------- |
| `main` (HEAD) | :white_check_mark: |
| Latest tagged release | :white_check_mark: |
| Prior releases | :x: |

The on-chain Soroban contracts are versioned independently per governance module. Deployed contract addresses and their corresponding commit hashes are tracked in each `onym-sep-*` repository.

## Reporting a Vulnerability

**Do not open a public GitHub issue for security reports.**

Please report vulnerabilities via one of:

1. **GitHub Private Vulnerability Reporting** — preferred. Use the "Report a vulnerability" button under the Security tab of the affected repository:
   - [onym-clients](https://github.com/onymchat/onym-clients/security) — iOS + Android apps
   - [onym-sdk-swift](https://github.com/onymchat/onym-sdk-swift/security)
   - [onym-sdk-kotlin](https://github.com/onymchat/onym-sdk-kotlin/security)
   - [onym-relayer](https://github.com/onymchat/onym-relayer/security)
   - [onym-sep-1v1](https://github.com/onymchat/onym-sep-1v1/security)
   - [onym-sep-anarchy](https://github.com/onymchat/onym-sep-anarchy/security)
   - [onym-sep-democracy](https://github.com/onymchat/onym-sep-democracy/security)
   - [onym-sep-tyranny](https://github.com/onymchat/onym-sep-tyranny/security)
   - [onym-sep-oligarchy](https://github.com/onymchat/onym-sep-oligarchy/security)
   - [onym-papers](https://github.com/onymchat/onym-papers/security)
   - [onym-docs](https://github.com/onymchat/onym-docs/security)

   If you are unsure which component is affected, file under [`.github`](https://github.com/onymchat/.github/security) and we will route it.

2. **Email** — `rinat.enikeev@gmail.com`. For sensitive reports, request a PGP key in your first message and we will reply with one before you send details.

Please include:

- Affected component (which repository, and the path inside it)
- Commit hash or release tag
- Reproduction steps or a proof-of-concept
- Impact assessment (e.g. membership deanonymization, proof forgery, key disclosure, denial of service)
- Whether the issue is already public or known to third parties

### In scope

- Soundness or zero-knowledge breaks in the Groth16 circuits of any governance module (`onym-sep-*`)
- Verifier bugs in any deployed Soroban contract (`onym-sep-*`), including BLS12-381 host-call misuse
- Epoch-ordering, replay, or commitment-binding flaws across any governance module
- Key handling issues in the Swift / Kotlin SDKs (`onym-sdk-swift`, `onym-sdk-kotlin`) and the client apps (`onym-clients`)
- Relayer vulnerabilities that leak fee-payer identity or enable request forgery (`onym-relayer`)
- Nostr transport issues that leak plaintext, leak metadata beyond what is documented, or break AES-256-GCM framing
- Build and release supply-chain issues (reproducibility, signing, distributed artifacts)

### Out of scope

- Traffic analysis against public Nostr relays (acknowledged limitation, see the org profile)
- Attacks requiring compromise of a member's device
- Recovery from BLS key compromise without re-keying (documented non-goal)
- Denial of service against self-hosted infrastructure that requires control of the network path
- Findings already disclosed in published audit reports or post-mortems within the affected repository

## Response Process

| Stage | Target |
| ----- | ------ |
| Acknowledgement of report | within 72 hours |
| Initial triage and severity assessment | within 7 days |
| Status update cadence during investigation | at least every 14 days |
| Fix, coordinated disclosure window, or decline with rationale | within 90 days of acknowledgement |

If the vulnerability affects a deployed Soroban contract or live relayer, a post-mortem will also be published in the affected repository's `docs/` directory after the fix is released.

## Disclosure

We prefer coordinated disclosure. Once a fix is released:

- A CVE will be requested for issues affecting deployed infrastructure or published SDKs.
- The reporter will be credited in the release notes and post-mortem unless they request anonymity.
- Proof-of-concept code and detailed technical write-ups may be published after users have had a reasonable window to upgrade (typically 30 days after the patched release).

## Safe Harbor

Good-faith security research against your own instances, test deployments, or the public testnet is welcome. Please do not:

- Access, modify, or destroy data belonging to other users
- Run denial-of-service or resource-exhaustion attacks against hosted infrastructure (`relay.onym.chat`, public Nostr relays we operate, or any production relayer)
- Exploit a vulnerability beyond what is necessary to demonstrate it

Researchers acting within these bounds will not be pursued under applicable computer-misuse laws by the maintainers.
