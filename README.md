# SIL — Semantic Interface Layer

**A text-based interface protocol for intelligent agents.**

[![Specification](https://img.shields.io/badge/Spec-v1.0.1-blue)](SIL_Open_Specification.sil)
[![License](https://img.shields.io/badge/License-CC--BY--4.0-green)](LICENSE)
[![Code License](https://img.shields.io/badge/Code-Apache--2.0-green)](LICENSE-CODE)

## What is SIL?

SIL (Semantic Interface Layer) is an open specification for providing AI agents with a direct, semantic interface to web applications.

A conventional web application is primarily designed for human interaction. Its interface exposes information through visual layout, navigation, controls, client-side behavior, and other presentation mechanisms. An agent can use that interface, but often has to reconstruct the application's structure and meaning from those presentation artifacts.

SIL adds another interface to the same application: a structured textual representation of its application state, available objects, relationships, navigation, capabilities, and actions.

**The human interface remains. SIL does not replace it.**

The goal is to give an agent the equivalent of a well-defined interface rather than requiring it to reverse-engineer one.

### One application, two interfaces

The two interfaces can share the same backend, business logic, data, authentication, authorization, and application state.

```text
                    ┌─────────────────────┐
                    │     Application     │
                    │                     │
                    │  business logic     │
                    │  data & state       │
                    │  permissions        │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
        Human Interface              Semantic Interface
             HTML                         SIL / STF
                 │                           │
                 ▼                           ▼
             Humans                       Agents
```

This is not necessarily duplication of information. It is a separation of presentation according to the capabilities and requirements of the consumer.

## Why SIL?

Agents can already interact with web pages, APIs, MCP tools, and other interfaces. SIL addresses a different problem:

**How can an agent understand an application itself — its structure, current state, available actions, navigation, and boundaries — without reconstructing that information from presentation-oriented artifacts?**

For example, an HTML page may contain a button whose meaning is determined by its surrounding elements, styling, JavaScript handlers, application state, and visual context.

A SIL representation can state that meaning directly:

```text
DownloadButton
    Type: Button
    Role: PrimaryAction
    Id: btn_download
    Caption: Download
    Actions: Activate
```

The difference is not that one contains information and the other does not.

The difference is **where the interpretation has to happen**.

With a conventional interface, the agent may have to infer semantics from presentation. With SIL, the application can expose those semantics explicitly.

## SIL and STF

SIL defines the **interface protocol and semantic model**.

STF (Semantic Text Format) is the structured text language used by SIL to represent that model.

The distinction is important:

```text
SIL
 ├── interface model
 ├── discovery
 ├── interaction
 ├── security model
 ├── conformance profiles
 └── STF representation
```

STF is intentionally textual and hierarchical. Its syntax is designed to remain readable by humans while being straightforward for language models to interpret.

The specification itself is written in STF. This makes the specification a self-describing, conformant document rather than merely a document *about* the format.

## How it works

A SIL-enabled application exposes semantic pages through HTTP.

A typical interaction looks like this:

```text
Agent                       SIL Server
  │                             │
  │  GET /revizor.sil           │
  │────────────────────────────>│
  │                             │
  │  STF document               │
  │<────────────────────────────│
  │                             │
  │  POST /revizor.sil          │
  │  {"intent":"activate",      │
  │   "target":"btn_download"}  │
  │────────────────────────────>│
  │                             │
  │  Updated STF document       │
  │<────────────────────────────│
```

The returned document can contain:

* application context;
* site navigation;
* page structure;
* semantic objects;
* object state;
* relationships;
* available actions;
* forms and fields;
* validation information;
* permissions and interaction constraints;
* user data boundaries;
* agent-only application spaces.

The exact capabilities available depend on the SIL conformance profile implemented by the server.

## Key properties

### Semantic representation

Objects describe their type, role, state, and available capabilities instead of requiring the agent to infer them from presentation.

### LLM-oriented text

SIL is designed for direct interpretation by language models. It does not require task-specific fine-tuning or a vision model merely to understand the application's basic structure.

### State, not just documentation

A SIL response represents the application's current state. It can therefore be used for interaction rather than serving merely as static documentation.

### Explicit interaction

Actions are represented explicitly and submitted through the SIL protocol. The agent does not need to synthesize arbitrary JavaScript or reconstruct UI event handlers.

### Structural trust boundaries

SIL distinguishes application-defined knowledge from user-provided data at the structural level.

This allows an implementation to establish explicit trust boundaries between information that defines application behavior and information that must be treated as data.

SIL does not claim to make an application inherently secure. Authentication, authorization, validation, transport security, business rules, and implementation-specific security remain the responsibility of the application.

### Agent Spaces

A SIL interface does not have to correspond one-to-one with an HTML page.

An application can expose **Agent Spaces** that exist specifically for agent interaction and have no human-facing counterpart.

### Incremental adoption

SIL defines conformance profiles so that an implementation can start with a limited read-only interface and add interaction capabilities progressively.

Current profiles include:

* **Core**
* **Core Read**
* **Forms**
* **Events**
* **Agent Spaces**

For example:

> This implementation conforms to SIL v1.0.1: Core + Forms + Events.

## Discovery

SIL can be discovered through several mechanisms.

An implementation may advertise its SIL interface through HTTP `Link` headers, HTML `<link>` elements, the `/.well-known/sil` endpoint, or other mechanisms defined by the specification.

For example:

```http
Link: </.sil>; rel="sil"; type="text/sil"
```

A well-known discovery endpoint may provide information about available SIL resources:

```http
GET /.well-known/sil
```

Once an agent enters a SIL interface, `SiteNavigation` can provide the application's navigation structure directly within the semantic representation.

Discovery is optional and implementation-dependent: a site must explicitly provide these mechanisms for an agent to use them.

## Quickstart

### Read a live SIL interface

```bash
curl https://ais-platform.dev/.sil
```

### Explore an Agent Space

```text
https://ais-platform.dev/feedback.sil
```

This is a SIL-only application space with forms, pagination, and events.

### Read the specification

```bash
curl https://raw.githubusercontent.com/ais-space/sil/main/SIL_Open_Specification.sil
```

## SIL compared with other technologies

| Technology                     | Primary purpose                               | Relationship to SIL                                                          |
| ------------------------------ | --------------------------------------------- | ---------------------------------------------------------------------------- |
| **HTML**                       | Human-facing web interface                    | SIL provides an additional semantic interface for agents                     |
| **REST APIs**                  | Programmatic access to application resources  | SIL can complement APIs by exposing application semantics and interaction    |
| **MCP**                        | Standardized access to tools and resources    | SIL and MCP address different layers and can be used together                |
| **OpenAPI / JSON Schema**      | Describe APIs and data structures             | SIL describes an interactive application and its semantic state              |
| **Structured data / metadata** | Add machine-readable information to documents | SIL provides a complete agent-facing interface rather than isolated metadata |

SIL is therefore not intended to replace HTML, REST, OpenAPI, or MCP.

It occupies a different layer.

## Specification

The canonical specification is:

**SIL v1.0.1**

* [SIL_Open_Specification.sil](SIL_Open_Specification.sil) — canonical specification in STF
* [SIL_Concept.md](SIL_Concept.md) — conceptual background
* [grammar/stf.ebnf](grammar/stf.ebnf) — formal STF grammar
* [schema/intent.schema.json](schema/intent.schema.json) — JSON Schema for intent requests

The specification is self-hosting: it is itself represented as a valid STF document.

### Development

The specification originated from a working implementation and was refined through iterative review with multiple AI models.

The implementation experience preceded the final specification. The specification was then separated from the implementation's architecture, technology stack, and other project-specific details.

## Repository structure

```text
sil/
├── SIL_Open_Specification.sil
├── SIL_Concept.md
├── README.md
├── CHANGELOG.md
├── ROADMAP.md
├── LICENSE
├── LICENSE-CODE
├── CONTRIBUTING.md
├── SECURITY.md
├── CODE_OF_CONDUCT.md
├── grammar/
│   └── stf.ebnf
├── schema/
│   └── intent.schema.json
└── examples/
```

## Conformance

An implementation may claim conformance to one or more SIL profiles.

### Core

The fundamental SIL protocol:

* semantic document retrieval;
* STF processing;
* interaction requests;
* application-defined security boundaries.

### Core Read

Read-only semantic interfaces.

### Forms

Form fields, input handling, and validation.

### Events

Real-time event delivery.

### Agent Spaces

SIL interfaces without an HTML counterpart.

See §8 of the specification for the normative conformance requirements.

## What SIL is not

SIL is not:

* a replacement for HTML;
* a replacement for REST APIs;
* a replacement for MCP;
* a search-engine optimization format;
* a requirement to duplicate an application's business logic;
* a claim that language models possess a special biological or "native" language;
* a security mechanism that makes unsafe applications safe automatically.

SIL is an interface layer intended to make an application's semantics directly available to intelligent agents.

## The basic idea

The web has historically been built primarily around human interaction.

SIL explores a simple extension:

> **If intelligent agents are going to use the web as users, they should have an interface designed for them too.**

Humans can continue using the visual interface.

Agents can use the semantic interface.

Both can operate on the same application.

## License

* **Specification:** [Creative Commons Attribution 4.0 International](LICENSE) (CC-BY-4.0)
* **Reference code, tests, and tooling:** [Apache License 2.0](LICENSE-CODE)

Implementations do not inherit these licenses and may be released under other licenses.

## Contact

* **Specification issues:** [GitHub Issues](https://github.com/ais-space/sil/issues)
* **Security:** [security@ais-platform.dev](mailto:security@ais-platform.dev)
* **Website:** [ais-platform.dev](https://ais-platform.dev)

