# Roadmap

## v1.0.0 (Released 2026-08-08) — First Stable Release

- [x] Core Profile: STF grammar, GET/POST, Actions, ActionError, security model
- [x] Core Read Profile: read-only semantic documents
- [x] Forms Profile: Input, SubmitForm, Expects, validation
- [x] Events Profile: response_mode, event types, SSE, WebSocket
- [x] Agent Spaces Profile: SIL-only pages
- [x] Structural security model (grammar-level trust boundary)
- [x] 18 error codes, 40 test vectors
- [x] Self-hosting specification

## v1.1.0 (Released 2026-08-16) — Hard Split: STF Core Extracted

- [x] STF core removed from SIL and published as a standalone specification (github.com/ais-space/stf, v0.1.0)
- [x] SIL references STF core normatively, without version pinning
- [x] New §2 "STF Core (Normative Reference)"; §3 SIL Vocabulary reframed as extension of STF core
- [x] `grammar/stf.ebnf` tracks STF core (GrammarVersion 1.0.1, `CHAR` terminal)
- [x] Specification reduced from 3,305 to 2,465 lines (no application-level changes)

## v1.1 (planned — post-split)

- [ ] CORS section in HTTP Binding
- [ ] Client Profile (minimal client: parse STF + send Intent + handle ActionError)
- [ ] IANA registration process for `text/sil` media type
- [ ] JSON Schema for Intent POST body
- [ ] End-to-end interaction example in documentation
- [ ] Machine-readable conformance test suite (runnable tests from Appendix A)
- [ ] TextMate/tree-sitter grammar for `.sil` syntax highlighting
- [ ] Governance model: contribution process documented

## v1.2 (planned)

- [ ] StateVersion promoted to Core Profile (currently experimental)
- [ ] Authenticated SIL (draft): user-bound sessions, access-controlled Agent Spaces
- [ ] Streamable STF: chunked transfer encoding for large documents
- [ ] Client Profile finalized
- [ ] Privacy section for public Agent Spaces
- [ ] AgentGeneratedContent role for LLM-produced content
- [ ] External Navigate behavior defined (200 + NavigationRequested event vs 3xx)
- [ ] POST body size limit in Resource Limits

## v2.0 (future)

- [ ] Freeze Core Profile with formal stability guarantee
- [ ] SIL Mirror vs SIL Native formal distinction
- [ ] Observation / Request / Authority as central design principle
- [ ] MCP integration patterns documented
- [ ] Binary STF encoding for minimal token usage
- [ ] SIL browser — human UI for navigating SIL documents
- [ ] Formal verifier for SIL implementations

## Continuous

- [ ] Community feedback via GitHub Issues and `/feedback.sil`
- [ ] Independent implementations and interoperability testing
- [ ] Security reviews and penetration testing
