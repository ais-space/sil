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

## v1.1 (planned)

- [ ] CORS section in HTTP Binding
- [ ] Client Profile (minimal client: parse STF + send Intent + handle ActionError)
- [ ] IANA registration process for `text/sil` media type
- [ ] JSON Schema for Intent POST body
- [ ] End-to-end interaction example in documentation
- [ ] Machine-readable conformance test suite (runnable tests from Appendix A)
- [ ] Formal grammar file (`.ebnf`) as separate artifact
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
