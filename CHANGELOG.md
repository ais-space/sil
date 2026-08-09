# Changelog

All notable changes to the SIL Specification.

---

## v1.0.0 (2026-08-08) — First Stable Release

### Core
- First stable release of the SIL specification
- Protocol version: 0.3
- 3305-line self-hosting STF document
- 18 error codes, 5 conformance profiles (Core, Core Read, Forms, Events, Agent Spaces)
- 40 conformance test vectors in Appendix A

### What Changed from v0.2
- **Formal STF grammar**: three-level (lexical → syntactic → semantic), parser/validator separation
- **Structural security model**: grammar-level trust boundary replaces string-filtering
- **Origin model**: fail-closed default (Unknown), explicit precedence order, privilege lattice
- **Field classification**: Data / Guidance / Control (renamed from Executable)
- **Three-layer architecture**: STF Syntax / Semantic Vocabulary / SIL Protocol — explicitly separated
- **Agent Spaces**: SIL-only pages without HTML counterparts
- **Knowledge Zones**: ApplicationKnowledge / UserData / AgentInstruction with structural boundaries
- **Conformance profiles**: Core, Core Read, Forms, Events, Agent Spaces — incremental adoption
- **Pagination**: stateless cursor, INVALID_CURSOR error code
- **Protocol versioning**: Sil-Version / Accept-Version headers
- **StateVersion**: experimental, view/interaction/domain state distinction
- **Idempotency**: HTTP header, mutation identity, priority over StateVersion
- **HTTP Status Reference**: authoritative table of all protocol responses
- **Context Block Reference**: consolidated index of all Context properties
- **Agent Error Recovery**: documented recovery patterns for agents
- **AgentInstruction**: explicit non-authority, no capability grant
- **URL security**: whitelist (http/https/absolute paths), relative paths rejected
- **Semantic injection**: honest scope — structural only, not semantic

### Specification Quality
- 10+ rounds of review by flagship AI models (AIS Agora methodology)
- All known contradictions resolved
- Section numbering validated, cross-references verified
- Self-hosting: specification is a valid STF document under its own grammar

---

## v0.2 (2026-08-06) — Public Draft

- Initial public draft with 8 modules, 21 pages
- STF format, Intent protocol, session model
- SSE + WebSocket transport
- Security model with Origin and string-based injection prevention
- Open specification v0.2 (EN + RU)

---

## v0.1 (2026-07) — Internal Working Draft

- Initial internal specification
- Working implementation on AIS Platform
- First SIL-only page: /feedback.sil
