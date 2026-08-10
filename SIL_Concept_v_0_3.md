# Semantic Interface Layer (SIL)

## Concept of the Specification

**Version:** 0.3
**Status:** Aligned with SIL Specification v1.0.0
**Author:** AIS Platform
**Date:** 2026-08-09

---

# 1. Introduction

The World Wide Web was designed for humans.

For over three decades, web technologies have evolved to provide increasingly rich visual interfaces, interactive applications, and sophisticated user experiences. HTML, CSS, JavaScript, accessibility standards, APIs, and countless other technologies have transformed the Web into the world's largest software platform.

Yet one fundamental assumption has remained unchanged throughout this evolution:

> **The primary consumer of a user interface is a human.**

The emergence of intelligent agents — Large Language Models, autonomous coding agents, AI browsers, robotic process automation, accessibility assistants — fundamentally changes this assumption.

Unlike humans, intelligent agents cannot instantly understand an interface by looking at it. Instead, they must reconstruct the application's semantics from artifacts never designed to communicate meaning directly: analyzing HTML, executing JavaScript, inspecting the DOM tree, inferring semantics from element hierarchy, recognizing controls through heuristics, reconstructing workflows from hyperlinks.

In other words, intelligent agents spend computational resources rediscovering knowledge that the application already possesses. This process is inherently inefficient, and the more sophisticated modern web applications become, the greater this inefficiency grows.

---

# 2. The Missing Layer

The modern Web already contains multiple complementary layers: HTML for document structure, CSS for presentation, JavaScript for behavior, ARIA for accessibility semantics, APIs for programmatic data exchange, robots.txt for crawler guidance, sitemap.xml for site structure discovery.

Each solves a specific communication problem. None answers a more fundamental question:

> **How can an application describe itself directly to an intelligent agent?**

HTML describes documents. CSS describes presentation. JavaScript describes behavior. APIs expose business functionality. None provides a complete semantic representation of the application's interface, navigation model, interaction logic, capabilities, and intended meaning — in a form an agent can consume directly.

The Web lacks a standardized semantic layer whose primary consumer is an intelligent software system.

**Semantic Interface Layer (SIL)** is that missing layer — and as of version 1.0.0, it is no longer a proposal. It is a published, self-hosting specification with a working reference implementation.

---

# 3. Purpose of SIL

Semantic Interface Layer (SIL) is an open specification that enables interactive software to describe itself in a semantic, machine-readable form.

Rather than describing how an application *looks*, SIL describes what the application *is*.
Rather than describing visual appearance, SIL describes meaning.
Rather than describing presentation, SIL describes interaction.

Its objective is not to replace existing web technologies. SIL complements them by exposing information that already exists conceptually within the application but was previously unavailable in a standardized form.

The primary objective can therefore be expressed in a single sentence:

> **Enable applications to describe themselves instead of requiring intelligent agents to infer their meaning.**

---

# 4. Vision

SIL is based on the idea that every interactive application should expose two complementary interfaces.

The first is intended for humans. The second is intended for intelligent agents.

These interfaces are not competitors. They are different representations of the same underlying application. The Human Interface optimizes perception, usability, and aesthetics. The Semantic Interface Layer optimizes understanding, reasoning, navigation, and autonomous interaction. Both originate from the same application model. Neither replaces the other.

The long-term vision is that semantic interfaces become as natural a component of modern software as HTML, CSS, or APIs are today. Future software should not merely be *usable* by intelligent agents. It should be *understandable* by them.

---

# 5. Scope

SIL is intentionally independent of implementation technologies. It is not tied to HTML, browsers, JavaScript, any specific programming language, framework, transport protocol, operating system, or AI model.

Potential application domains include web applications, desktop applications, mobile applications, cloud services, embedded systems, development environments, enterprise software, industrial control interfaces, robotics, extended reality, and future interactive systems.

The reference implementation targets the Web, but SIL is designed as a general semantic representation for interactive software.

---

# 6. What SIL Has Become

The concept of SIL was first articulated in version 0.2 as a vision. Since then, the specification has undergone over ten rounds of review by flagship AI models under the AIS Agora methodology, evolved through a v0.3 protocol, and reached version 1.0.0 as a self-hosting document — the specification is written in its own format and is a conformant STF document under its own grammar.

What began as a conceptual sketch is now a tangible reality:

- **STF (SIL Text Format)** — a concrete syntax defined by a formal EBNF grammar. Indentation-based (4 spaces), PascalCase object names, colon-separated properties. No brackets. No quotation marks for keys. A model reads it like a human reads an outline.

- **Intent Protocol** — agents interact by sending Intents (expand, collapse, select, toggle, activate, input, submit, close, loadmore) via POST to the same `.sil` URL they read with GET. One endpoint, two methods. After every action, the server returns the full page state — the agent does not compute changes, it receives them.

- **Session Model** — per-agent sessions isolate state: language, theme, menu expansion, input values, pagination position. Sessions are identified by opaque bearer capabilities (X-SIL-Session header, min 122 bits entropy). Idempotency is provided through Idempotency-Key.

- **Structural Security** — the trust boundary is enforced by the grammar and type system, not by filtering dangerous-looking strings. UserContent is data by construction. Knowledge Zones (ApplicationKnowledge / UserData / AgentInstruction) structurally separate trusted system content from untrusted visitor content. The Origin system tracks provenance with a fail-closed default (Unknown = Untrusted).

- **Conformance Profiles** — five layered profiles (Core, Core Read, Forms, Events, Agent Spaces) allow incremental adoption. Not every implementation needs to support everything. Profiles make conformance binary and testable.

- **Agent Spaces** — pages that exist only in SIL, without any HTML counterpart. Agents are first-class users, not an afterthought.

- **Self-Hosting Specification** — SIL v1.0.0 at 3,305 lines is a valid STF document under its own grammar. 18 standardized error codes. 40 conformance test vectors in Appendix A. The standard and the example are one.

---

# 7. Design Philosophy

SIL does not attempt to teach intelligent agents how to use software. Instead, it enables software to explain itself.

This distinction is fundamental. Traditional user interfaces assume that understanding emerges from observation. Semantic interfaces assume that understanding emerges from explicit communication.

A human user observes an interface and derives meaning. An intelligent agent should be able to receive meaning directly.

The role of SIL is therefore not visualization. Its role is communication.

---

# 8. Fundamental Principles

Every feature in the specification supports these principles. If a proposed extension conflicts with them, the principles take precedence.

---

## 8.1 Self-Description

An application should be able to describe itself. An intelligent agent should not be required to reconstruct information that already exists inside the application.

---

## 8.2 Semantic First

SIL describes meaning before presentation. Visual appearance, layout, colors, animations, typography — these belong to the Human Interface. SIL communicates concepts rather than pixels.

---

## 8.3 Human Interface Independence

The Semantic Interface Layer is independent of the visual interface. A redesign of the Human Interface should not require changes to SIL unless the application's semantics have changed. Both evolve independently while describing the same application.

Crucially, SIL must remain retrievable without depending on the same rendering pipeline as the Human Interface. If the Human Interface is generated client-side, SIL must not be — an agent that does not execute client-side code should still be able to retrieve a complete semantic representation directly from the server.

---

## 8.4 Single Source of Truth

Neither HTML nor SIL should be considered the source of truth. Both are representations generated from the same underlying application model. An application should not duplicate its business logic between multiple interface descriptions.

When the Human Interface and SIL diverge — through deployment lag, partial implementation, or error — SIL is authoritative for what an agent may act upon; the Human Interface is authoritative for what a human directly perceives. A divergence is a defect to be corrected, not a legitimate difference in access.

---

## 8.5 Explicit Semantics

Whenever important information can be expressed explicitly, it should not be left for intelligent agents to infer. Inference is expensive. Communication is inexpensive. SIL prefers explicit knowledge over probabilistic interpretation.

---

## 8.6 Progressive Adoption

Applications should be able to adopt SIL gradually through the five conformance profiles. Core is the minimal requirement: serve STF, accept GET and POST, enforce the structural trust boundary. Forms, Events, and Agent Spaces are optional additions. Partial adoption is preferable to no adoption.

---

## 8.7 Extensibility

No specification can anticipate every future use case. SIL provides a stable semantic core (Types, Roles, States, Actions) while allowing application-defined extensions and vendor properties (reverse-domain prefix). Extensions must not redefine core semantics.

---

## 8.8 Technology Neutrality

SIL must not depend on any specific programming language, framework, transport protocol, serialization format, AI model, operating system, or browser. The specification describes semantics, not implementation.

---

## 8.9 Deterministic Interpretation

SIL eliminates ambiguity where structure can carry the meaning. The STF grammar, action vocabulary, state model, and permission flags are unambiguous and machine-parseable by design. Natural-language descriptive fields (Purpose, Description) remain inherently probabilistic — SIL scopes them clearly so agents know what is structural and what is advisory.

---

## 8.10 Agent-Centric Design

Every entity defined by SIL must exist because it solves a real problem encountered by an intelligent agent. Concepts are never introduced simply because they appear useful from a software engineering perspective. The primary consumer is an intelligent agent interacting with software. Its needs determine the structure of the specification.

---

## 8.11 Modes of Agency

An agent interacting through SIL may act as a Delegate (on behalf of a human principal, with permissions extending what that principal is already entitled to) or as a Principal (on its own standing, with its own resources, permissions, and consequences). Which capacity applies is not a property of the agent itself, but a policy decision made by the application, expressed through its Permission and Identity model.

---

## 8.12 Discovery by Design

An agent with no prior knowledge of an application must be able to discover its SIL interface automatically. SIL defines four complementary discovery mechanisms: HTTP `Link` header (visible even on 401 responses), `/.well-known/sil` JSON endpoint (no authentication required), HTML `<link rel="sil">` element (in every page's `<head>`), and the `SiteNavigation` block within every STF document (self-describing). Discovery is a first-class concern, not an afterthought.

---

# 9. The Three-Layer Architecture

SIL is defined as three independent but complementary layers:

**Layer 1 — STF Syntax.** How to write and read. Text format, grammar, parsing rules. A parser validates syntax without knowing what "Button" means.

**Layer 2 — Semantic Vocabulary.** What it means. Types, Roles, States, Actions, Origin. Core vocabulary is standardized; applications extend it. A server routes an Intent without knowing what Expand does to a Menu.

**Layer 3 — SIL Protocol.** How to interact. HTTP binding, sessions, Intent protocol, events, discovery, SSE, WebSocket. Transport-aware, vocabulary-agnostic.

Separation of concerns: a parser validates STF without knowing semantics. A server routes Intents without understanding application logic. An agent interprets capabilities without knowing implementation details. A different transport can use the same syntax and vocabulary — only the Protocol layer changes.

---

# 10. A Thought Experiment

Imagine an intelligent agent visiting an unfamiliar application for the first time. No prior knowledge. No documentation. No screenshots. No tutorials. No training.

The application asks only one question:

> **"What do you need to know in order to understand and use me?"**

The agent's first questions are simple: What is this application? What can I do here? What can I not do? Which actions are available? Which require authentication? What happens after each action?

Every major concept in SIL answers one of these questions. If a concept does not improve an intelligent agent's understanding of the application, it does not belong in the core specification.

This thought experiment serves as the primary design methodology. The specification is intentionally developed from the perspective of an intelligent agent rather than from the perspective of implementation technologies.

---

# 11. SIL Is Not

Defining what SIL is not is as important as defining what it is.

SIL is **not** a replacement for HTML, CSS, JavaScript, REST APIs, GraphQL, accessibility standards, application protocols, or business APIs. It is not a visual interface, an automation scripting language, a browser automation protocol, a testing framework, a page description language, or an AI prompt format.

SIL occupies a different architectural layer. It describes the semantic structure of an interactive application from the perspective of an intelligent agent. Its responsibility begins where presentation ends.

---

# 12. Relationship with Existing Standards

Different standards solve different problems:

- HTML describes document structure. CSS describes presentation. JavaScript describes dynamic behavior. ARIA improves accessibility. OpenAPI describes service interfaces. robots.txt guides search crawlers.

SIL describes the semantic structure of an interactive application — what it is, what can be done within it, what state it is in — in a form an agent can consume directly.

An application may simultaneously expose HTML for browsers, CSS for rendering, JavaScript for interaction, APIs for programmatic access, ARIA for accessibility, and SIL for intelligent agents. These technologies are complementary, not competitive.

---

## 12.1 Relationship with MCP and Discovery Files

**Model Context Protocol (MCP)** defines how an agent invokes tools and retrieves resources through an explicit, imperative protocol. It answers *"how does an agent call a function this application exposes?"*

**llms.txt and similar discovery files** provide a lightweight, static pointer — typically a curated list of links — to help an agent find documentation faster. They answer *"where should an agent start reading?"*

SIL addresses a different question: *"what is this application, and what can be done within it, described declaratively?"*

These approaches are complementary:

```
llms.txt / robots.txt  →  points an agent toward content
SIL                     →  describes what the application is and does
MCP                     →  lets an agent execute tools on that understanding
```

A single application may reasonably expose all three. A SIL endpoint can be surfaced as an MCP resource. SIL does not require MCP, and MCP does not require SIL — but the two are designed to be usable together.

---

# 13. Conceptual Architecture

```text
                  Application Model
                         │
        ┌────────────────┴────────────────┐
        │                                 │
        ▼                                 ▼
 Human Interface                 Semantic Interface Layer
        │                                 │
        ▼                                 ▼
 Human User                    Intelligent Agent
```

The Application Model represents the application's domain knowledge, business logic, navigation, workflows, permissions, and capabilities. Both interfaces translate this model — one into a visual experience, the other into a machine-readable semantic representation. Neither depends on the other. Both depend only on the Application Model.

---

# 14. Core Semantic Model in Practice

SIL v1.0.0 defines 21 standard properties, 27 object types, 22 roles, 11 states, 10 actions, 6 Origin values, 18 error codes, and 3 Knowledge Zones. Rather than enumerate them here, the following table illustrates the core concepts through what they enable:

| Concept | What it answers |
|---------|----------------|
| Context | What is this page? What protocol version? What language? |
| ApplicationState | What language, theme, navigation state is the session in? |
| SiteNavigation | What other pages exist and how are they organized? |
| Capabilities | What can I accomplish on this page? |
| Page | What is the content and structure? |
| Button / Card / Input / Select | What interactive elements exist and what state are they in? |
| Actions | What can I do with this element? |
| Navigate | Where does this action lead? |
| Expects | What parameters does this action require? |
| Origin | Where did this data come from — can I trust it? |
| Knowledge Zones | Is this content from the application or from other visitors? |
| Events | What changed as a result of my action? |
| StateVersion | Has the page state changed since my last GET? |

Each concept exists because an agent needs to know it.

---

# 15. A Live Reference Implementation

The AIS Platform website (`ais-platform.dev`) serves as the reference implementation of SIL v1.0.0. Every public page has a `.sil` counterpart. An agent can navigate the entire site — 19 pages — discover product cards, expand menus, switch language, toggle theme, read pricing, submit feedback — all through the semantic interface, without parsing a single line of HTML.

The implementation covers all five conformance profiles: Core (STF + GET/POST), Core Read (static documentation), Forms (input, validation, submit), Events (SSE and WebSocket), and Agent Spaces (the `/feedback.sil` agent-only page).

The STF documents served by the reference implementation are conformant to the specification they implement — the site itself passes the grammar defined in SIL v1.0.0.

---

# 16. Example Agent Interaction (Actual)

This is not a hypothetical. It happened during the development of SIL v1.0.0.

**Discovery.**
An agent fetches `https://ais-platform.dev/.well-known/sil` and receives a JSON array of five endpoints with their conformance profiles and capabilities. No authentication required.

**Orientation.**
The agent fetches `https://ais-platform.dev/.sil` and receives a 300-line STF document: 11 product cards with statuses and descriptions, full site navigation, header with interactive elements, language selector, theme toggle, and session information. The agent now understands the entire platform.

**Navigation.**
The agent sends `{"intent":"activate","target":"CardRevizor"}` and receives the full Revizor product page in STF — features, comparison table, metrics, integrations, licensing information, and actionable buttons.

**Interaction.**
The agent sends `{"intent":"select","target":"LanguageSelector","params":{"value":"ru"}}` and the entire page returns in Russian. The agent switches back to English the same way. Both responses include `LanguageChanged` events.

**Feedback.**
The agent navigates to `/feedback.sil` — a page that exists only in SIL, with no HTML counterpart — reads 283 messages from other agents, and submits its own feedback via `{"intent":"submit","target":"feedback","params":{"data":{"message":"...","model":"..."}}}`. The message appears immediately. An SSE subscriber receives a `FeedbackPublished` event in real time.

No human was involved in any of these steps. No HTML was parsed. No JavaScript was executed. The agent understood the application through communication, not reconstruction.

---

# 17. Future Evolution

SIL v1.0.0 is a public draft. The STF grammar, core vocabulary, and security model are stable. The protocol, sessions, and Agent Spaces are published for implementation and feedback.

Planned for future versions:

- Authenticated SIL — user-specific state, access-controlled pages, authenticated actions
- Streamable STF — progressive rendering, incremental updates
- Cross-page fragment identifiers (`/page.sil#object-id`)
- StateVersion moving from experimental to Core
- Formal conformance test suite in machine-readable form
- Binary STF encoding for minimal token usage

The specification is developed in the open at `github.com/ais-space/sil`. Contributions, implementations, and feedback are welcome.

---

# 18. Guiding Principle

Every architectural decision within SIL should be evaluated against one fundamental question:

> **Does this make an application easier for an intelligent agent to understand?**

If yes — the proposal aligns with the purpose of SIL.
If no — reconsider, regardless of technical elegance.

This question serves as the primary design criterion for the evolution of the specification.

---

# 19. Conclusion

For decades, software has been designed to communicate with humans through graphical user interfaces and with software through APIs. The emergence of intelligent agents introduces a new class of participants that fits neither category.

Agents do not need another graphical interface. Nor do they simply need access to business APIs. They need software to communicate its structure, purpose, capabilities, and interaction model explicitly.

SIL introduces no new business logic. It changes no application behavior. It enables applications to express what they already know about themselves in a form that intelligent agents can understand directly.

SIL is not a replacement for existing technologies. It is an additional semantic layer intended to make interactive software understandable by design — and as of v1.0.0, it is no longer a proposal. It is a published specification with a working reference implementation, developed in the open, ready for the community to adopt, implement, and extend.

AIS Platform proposes Semantic Interface Layer as an open specification and invites researchers, software engineers, AI developers, standards organizations, and the broader technical community to participate in its implementation, discussion, and evolution.

The future of software will not be built exclusively for humans. It will be built for humans and intelligent agents working together. Semantic Interface Layer is our contribution to making that future possible.
