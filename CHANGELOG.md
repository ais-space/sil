# Changelog

All notable changes to the SIL Specification.

---

## v1.1.2 (2026-11-21) — Reference site offline note

- **Documentation update**: removed live quickstart links to `ais-platform.dev` in `README.md` and marked the reference implementation in `SIL_Concept.md` (§15–§16) as an offline historical record (offline since August 2026). No specification content changed.

## v1.1.1 (2026-10-06) — Contact correction

- **Contact address fixed**: the `security@ais-platform.dev` mailbox does not exist; replaced with `support@ais-platform.dev` in `README.md` and `SECURITY.md`. No specification content changed.

## v1.1.0 (2026-08-16) — Hard Split: STF Core Extracted to Standalone Normative Reference

This release restructures the specification without changing any application-level (SIL) content. It is backwards compatible for all SIL implementations.

### Structural change
- **Removed the inline STF core from SIL.** The syntax, semantic model, structural-security model, and data/serialization model are no longer defined inside the SIL specification.
- **STF core is now a separate, standalone specification**, published independently at [github.com/ais-space/stf](https://github.com/ais-space/stf) (tag v0.1.0).
- **SIL references STF core normatively, by reference, without version pinning.** Any SIL document is, by construction, a valid STF document; SIL tracks core evolution at the application layer.
- New §2 "STF Core (Normative Reference)" replaces the former §2 STF Syntax. SIL Vocabulary (§3) is now explicitly framed as an extension of STF core. Application Vocabulary, SIL Protocol, Interaction Patterns, Security-specific, Conformance, and Extensions (§4–§9) are unchanged in substance.

### Grammar
- `grammar/stf.ebnf` now tracks STF core (GrammarVersion 1.0.1), including the `CHAR` terminal; header adjusted to mark it as extracted from STF core with no version pin.

### Compatibility
- No SIL application-vocabulary, protocol, or grammar changes.
- Fully backwards compatible (the split removes duplicated core material; SIL semantics are preserved).
- Specification reduced from 3,305 to 2,465 lines by removing the duplicated core.

### Notes
- **Origin semantics (correction, not a vocabulary change).** The former inline SIL §3.8 description of Origin propagation was defective: it treated `UserContent` as defaulting to `External` and used `effective = min(explicit, zone)`. SIL no longer defines Origin; it references the STF core Trust Model (STF core §4.2.2), which defines the corrected propagation (`UserContent` is always untrusted; `effective = min(base_trust, zone_max_trust)`). This corrects a defective SIL-internal description; SIL's application vocabulary is unchanged.
- **`Format` behavior.** The `Format` property's normative behavior (all values transmitted as text; `string` assumed when `Format` is absent; implementations SHOULD respect `Format` when validating input) and the per-format examples are defined in STF core §3.2.1. SIL references them by reference; this material is STF core semantics, not SIL-specific.
- **`Contact` property restored.** The `Contact` property (SIL-specific, Data class) for `UserContent` was omitted when the STF core was extracted, but the live SIL implementation (e.g. `/feedback.sil`) uses it. It is now defined in §3.1 SIL-Specific Properties and listed among the typical `UserContent` properties in §3.2.

---

## v1.0.1 (2026-08-15) — Documentation & Nomenclature Corrections

### Corrections
- Fixed erroneous expansion of the STF acronym: "SIL Text Format" → "Semantic Text Format" across the specification, grammar, and concept documents
- Removed version-narrative artifacts (v0.4 roadmap references, v0.3 historical asides) per the document versioning policy; the version is now stated exactly once in the document meta-block
- Renamed canonical files to version-free permanent names: `SIL_Open_Specification.sil`, `SIL_Concept.md`

### Compatibility
- No protocol or grammar changes; wire protocol remains SIL 0.3, STF grammar 1.0
- Fully backwards compatible (patch release)

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
