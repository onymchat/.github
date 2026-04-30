# Contributing

Thanks for your interest in Onym. Contributions are welcome across the org.

This guide applies to every repository under [`onymchat`](https://github.com/onymchat). Per-repo `CONTRIBUTING.md` files override these defaults when present.

## How to contribute

1. **Open an issue** in the relevant repository to report a bug, request a feature, or discuss a change before starting work on anything non-trivial. If you are not sure which repo applies, open it in [`.github`](https://github.com/onymchat/.github/issues) and we will route it.
2. **Open a pull request** against `main` of that repo with your change. Link the issue it closes or relates to.

That's it. No CLA, no style committee, no required templates beyond what each repo ships.

## Where things live

| Component | Repo |
| --------- | ---- |
| iOS app | [`onym-ios`](https://github.com/onymchat/onym-ios) |
| Android app | [`onym-android`](https://github.com/onymchat/onym-android) |
| Swift SDK | [`onym-sdk-swift`](https://github.com/onymchat/onym-sdk-swift) |
| Kotlin SDK | [`onym-sdk-kotlin`](https://github.com/onymchat/onym-sdk-kotlin) |
| Relayer | [`onym-relayer`](https://github.com/onymchat/onym-relayer) |
| Soroban contracts (one per governance shape: 1v1, anarchy, democracy, tyranny, oligarchy) | [`onym-contracts`](https://github.com/onymchat/onym-contracts) |
| Papers | [`onym-papers`](https://github.com/onymchat/onym-papers) |
| Docs | [`onym-docs`](https://github.com/onymchat/onym-docs) |

## Using AI

AI-assisted contributions are welcome. Claude Code, Copilot, Cursor, and similar tools are all fine. A few expectations:

- **You are responsible for every line you submit.** Read the diff, understand it, and make sure it does what you think it does. "The model wrote it" is not a defense for a broken PR.
- **Test it.** Run the affected tests locally — `cargo test` for the Rust circuits / contracts / relayer, the Swift/Kotlin test targets for the SDKs and clients. Do not rely on CI to catch basic breakage.
- **Don't submit AI slop.** Generated docs, README fluff, speculative refactors, or changes that "improve" working code without a concrete reason will be closed.
- **Security-sensitive code needs extra care.** Circuits, verifier logic, key handling, and the relayer are not places to accept model output uncritically. If an AI wrote it, review it twice.

## What to keep in mind

- **Scope:** Onym is a privacy-critical cryptographic system. Small, focused PRs land faster than large ones. If your change touches the circuits, verifier, or epoch logic, expect a slower review.
- **Cross-repo changes:** if a change spans multiple repos (e.g. SDK + client app, or a contract update in `onym-contracts` paired with an SDK change), open the PRs together and link them in each description.
- **Documentation:** if you change behavior documented in [`onym-docs`](https://github.com/onymchat/onym-docs) or in a paper under [`onym-papers`](https://github.com/onymchat/onym-papers), update the doc in the same PR (or open a paired PR there and link it).
- **Security issues:** do not file them as public issues or PRs. See [`SECURITY.md`](SECURITY.md).

## License

By contributing, you agree that your contribution is licensed under the license of the repository you are contributing to. See the `LICENSE` file in that repo.
