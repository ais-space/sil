# Contributing to SIL

Thank you for your interest in contributing to the Semantic Interface Layer (SIL) specification and ecosystem.

## Ways to Contribute

### 1. Specification Feedback
- Open an issue for: bugs, ambiguities, missing edge cases, unclear wording
- Proposals for new features or changes: open an issue first to discuss before submitting a PR
- See `CHANGELOG.md` for the history of changes

### 2. Implementation Experience
- Build a SIL parser, server, or client
- Report interoperability issues you encounter
- Share your implementation in the [Implementations](https://github.com/ais-space/sil) list

### 3. Test Vectors
- Submit new test vectors that cover edge cases
- Help build the machine-readable conformance test suite
- See Appendix A of the specification for the current test vector format

### 4. Documentation
- Improve the README, examples, or guides
- Translate the specification (coordinate via issue first)
- Create tutorials or blog posts

## Pull Request Process

1. Fork the repository
2. Create a branch for your change
3. Ensure your change follows the repository structure
4. For specification changes: update the relevant `.sil` part in `spec/` and rebuild
5. For new test vectors: follow the format in Appendix A
6. Submit a PR with a clear description of the change and its motivation

## Specification Development

The canonical specification is `SIL_Open_Specification.sil` — a self-hosting STF document, itself a valid STF document under its own grammar. Proposed changes are discussed through issues and pull requests; a change should keep the specification internally consistent and self-validating under its grammar.

## Governance

This project is maintained by AIS Platform. Specification changes are proposed through GitHub issues and pull requests, reviewed by maintainers, and merged when consensus is reached. For the current status of specification components, see §9.1 (Component Stability) of the specification.

## License

- Specification text: [CC-BY-4.0](LICENSE)
- Reference code and tests: [Apache-2.0](LICENSE-CODE)
- Contributions are accepted under the same terms
