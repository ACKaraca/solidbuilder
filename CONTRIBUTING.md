# Contributing to SolidBuilder

SolidBuilder is part of the [Arc suite](https://github.com/ACKaraca/arc-suite). The suite-wide guide defines the
workflow, review process, sign-off and standards, and applies here in full:

- [Contribution guide](https://github.com/ACKaraca/arc-suite/blob/main/CONTRIBUTING.md)
- [Engineering standards](https://github.com/ACKaraca/arc-suite/tree/main/docs/engineering)
- [Clean-room policy](https://github.com/ACKaraca/arc-suite/blob/main/docs/engineering/clean-room-policy.md)
- [Code of conduct](https://github.com/ACKaraca/arc-suite/blob/main/CODE_OF_CONDUCT.md)

## Working in this repository

> **Current working mode.** Until the project has a second maintainer and this repository has
> reached its MVP release, changes are committed directly to `main` (suite rule 14): no feature
> branches and no pull requests. Where this document says "pull request", read "commit". The
> branch, review and merge rules below apply again when rule 14 is removed.

1. Start from an issue. Search open issues and pull requests first, comment on the issue you intend
   to take, and wait for it to be assigned before starting substantial work.
2. Read [docs/README.md](docs/README.md) for the documentation map. Capability identifiers used in
   issues and pull requests are defined under [docs/product](docs/product).
3. Keep each pull request to one concern, with the tests that prove it. Changes to behavior that
   is described in `docs/` update the description in the same pull request.
4. Record a decision that changes architecture, a file format or a public interface as an
   architecture decision record under [docs/adr](docs/adr).
5. Sign off every commit (`git commit -s`) to certify the
   [Developer Certificate of Origin](https://developercertificate.org/).

Automated coding agents follow [AGENTS.md](AGENTS.md); the person who runs an agent is accountable
for what it submits.

## License

Contributions are accepted under the [Apache License 2.0](LICENSE), the license of this repository.
Contributors keep their copyright; no copyright assignment is requested.
