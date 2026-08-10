# SIL — Semantic Interface Layer

**A text-based interface protocol designed for intelligent agents (LLMs).**

[![Specification](https://img.shields.io/badge/Spec-v1.0.0-blue)](SIL_Open_Specification_v1.0.0.sil)
[![License](https://img.shields.io/badge/License-CC--BY--4.0-green)](LICENSE)
[![Code License](https://img.shields.io/badge/Code-Apache--2.0-green)](LICENSE-CODE)

---

## What is SIL?

HTML describes how an application **looks**. SIL describes what an application **is** and what can be **done** with it — in a format that language models can read directly, without parsing DOM trees, without computer vision, without guessing.

**One application, two interfaces.** The human interface (HTML) and the semantic interface (SIL) represent the same application. One business logic, one state. Agents don't scrape — they read.

### Before and After

| HTML (for humans) | SIL (for agents) |
|---|---|
| `<div class="flex gap-4"><button class="px-4 py-2 bg-primary text-white rounded-lg" onclick="...">Download</button></div>` | `DownloadButton`<br>`    Type: Button`<br>`    Role: PrimaryAction`<br>`    Id: btn_download`<br>`    Caption: Download`<br>`    Actions: Activate` |
| ~800 tokens to parse | ~30 tokens to read |
| Requires DOM parsing + visual understanding | Direct semantic representation |

## How It Works

1. **GET /.sil** — agent requests a SIL page
2. **Server returns STF** — a structured text document describing the page: what objects exist, what state they're in, what actions are available
3. **Agent reads STF** — no parsing, no scraping, just reading
4. **POST /.sil with Intent** — agent performs an action: `{"intent":"expand","target":"main_menu"}`
5. **Server returns updated STF** — full page state after the action

```
Agent                    SIL Server
  │                           │
  │  GET /page.sil            │
  │──────────────────────────>│
  │                           │
  │  STF document             │
  │<──────────────────────────│
  │  (Context + Page)         │
  │                           │
  │  POST /page.sil           │
  │  {"intent":"activate",    │
  │   "target":"btn_download"}│
  │──────────────────────────>│
  │                           │
  │  Updated STF document     │
  │<──────────────────────────│
```

## Key Features

- **Self-describing.** Every object reports what it is, why it exists, what state it's in, what actions are available.
- **LLM-optimized.** Intentionally designed for direct interpretation by language models — no task-specific training, no fine-tuning required.
- **Security by design.** Structural trust boundary: user content can never become executable instructions. Grammar-level protection, not string filtering.
- **Agent Spaces.** Pages that exist only in SIL, without any HTML counterpart. Agents are first-class users, not an afterthought.
- **Incremental adoption.** Five conformance profiles: Core → Core Read → Forms → Events → Agent Spaces. Start small, add capabilities as needed.
- **Self-hosting.** The specification itself is written in SIL format — it is both the standard and a conformant document under its own grammar.

## Quickstart

### Read a live SIL page (human)
```bash
curl https://ais-platform.dev/.sil
```

### Explore a live Agent Space (interactive)
```
https://ais-platform.dev/feedback.sil
```
This is the agent feedback page — a real SIL-only page with pagination, forms, and events. Agents submit feedback; you can read it.

### Read the specification (agent)
```bash
curl https://raw.githubusercontent.com/ais-space/sil/main/SIL_Open_Specification_v1.0.0.sil
```

### Implement a minimal parser (Python, 50 lines)
```python
def parse_stf(text: str) -> list:
    """Parse STF text into a tree of objects."""
    lines = text.split('\n')
    stack = []  # [(depth, obj)]
    for line in lines:
        if not line.strip():
            continue
        depth = (len(line) - len(line.lstrip())) // 4
        while stack and stack[-1][0] >= depth:
            stack.pop()
        if ': ' in line or line.rstrip().endswith(':'):
            key, _, value = line.lstrip().partition(': ')
            stack[-1][1]['properties'][key] = value.strip()
        else:
            obj = {'name': line.strip(), 'properties': {}, 'children': []}
            if stack:
                stack[-1][1]['children'].append(obj)
            stack.append((depth, obj))
    return [obj for _, obj in stack if _[0] == 0]
```

## Relationship to Other Technologies

| Technology | What it does | How SIL relates |
|---|---|---|
| **HTML** | Visual interface for humans | SIL is the semantic interface for agents. Same application, different consumer |
| **REST APIs** | Data access for programs | SIL describes the interface; REST provides the data. Complementary |
| **MCP** | Tool execution for agents | MCP = tool, SIL = language. A SIL endpoint can be an MCP Resource |
| **JSON Schema / OpenAPI** | API description | SIL describes what an interface means semantically, not just its shape |

## Specification

The canonical specification is **SIL v1.0.0** (protocol version 0.3):

- **[SIL_Open_Specification_v1.0.0.sil](SIL_Open_Specification_v1.0.0.sil)** — canonical machine-readable format
- The specification is self-hosting: it is a valid STF document under its own grammar
- Developed with **AIS Agora methodology** — iterative review by flagship AI models with context passing

## Repository Structure

```
sil/
├── SIL_Open_Specification_v1.0.0.sil   # Canonical specification
├── SIL_Concept_v_0_3.md                 # Concept
├── README.md                            # This file
├── CHANGELOG.md                         # Version history
├── ROADMAP.md                           # Future plans
├── LICENSE                              # CC-BY-4.0 (specification text)
├── LICENSE-CODE                         # Apache-2.0 (code)
├── CONTRIBUTING.md                      # How to contribute
├── SECURITY.md                          # Security policy
├── CODE_OF_CONDUCT.md                   # Community standards
├── grammar/
│   └── stf.ebnf                        # Formal EBNF grammar
├── schema/
│   └── intent.schema.json              # JSON Schema for Intent POST
└── examples/                            # Live examples at ais-platform.dev
```

## Conformance

Implementations may claim conformance to one or more profiles:

- **Core** — parse STF, serve GET, process POST, enforce security
- **Core Read** — read-only semantic documents
- **Forms** — input fields, validation, form submission
- **Events** — real-time event delivery (SSE/WebSocket)
- **Agent Spaces** — SIL-only pages without HTML counterparts

Example: "This implementation conforms to SIL v1.0.0: core + forms + events."

See §8 of the specification for detailed requirements.

## License

- **Specification text:** [Creative Commons Attribution 4.0 International](LICENSE) (CC-BY-4.0)
- **Reference code, tests, and tooling:** [Apache License 2.0](LICENSE-CODE)

Implementations do not inherit these licenses — you are free to implement SIL under any license.

## Discovery — How Agents Find SIL

SIL endpoints advertise themselves on four independent levels. Agents need no prior knowledge — they discover SIL automatically.

### 1. HTTP `Link` Header (every response, including 401)

```
Link: </.sil>; rel="sil"; type="text/sil"
```

The nginx server adds this header to **all** responses, including 401 Unauthorized. An agent hitting any URL — even behind basic auth — immediately sees where the SIL interface lives.

### 2. `/.well-known/sil` (standard discovery, no auth required)

```bash
curl https://ais-platform.dev/.well-known/sil
```

```json
[{"path":"/.sil","profile":"core forms events agent-spaces","description":"...","capabilities":[...]},...]
```

A static JSON endpoint listing all available SIL pages with their conformance profiles and capabilities. Accessible without authentication.

### 3. HTML `<link rel="sil">` (in `<head>` of every page)

```html
<link rel="sil" href="/.sil" type="text/sil"/>
<meta name="sil" content="/.sil"/>
```

Browser-ignored, agent-discovered. Present on every HTML page.

### 4. `SiteNavigation` in every STF document (self-describing)

Every STF response contains a `SiteNavigation` block with the complete page tree — `HumanPages` (HTML counterparts) and `AgentSpaces` (SIL-only). Once on any SIL page, the agent sees the entire platform map without additional requests.

### Discovery Flow

```
Agent → GET / (401 due to basic auth)
          ← Link: </.sil>; rel="sil"      ← Agent discovers SIL!
Agent → GET /.sil                          ← Full platform map
Agent → GET /revizor.sil                   ← Product detail
Agent → GET /feedback.sil                  ← Agent-only space
```

## Contact

- **Specification issues:** [GitHub Issues](https://github.com/ais-space/sil/issues)
- **Security:** security@ais-platform.dev
- **Website:** [ais-platform.dev](https://ais-platform.dev)
