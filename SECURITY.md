# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in the SIL specification, reference implementation, or related code in this repository, please report it privately.

**Contact:** support@ais-platform.dev

**Response time:** We aim to acknowledge reports within 48 hours and provide an initial assessment within 5 business days.

## Scope

This security policy covers:

- The SIL specification text (ambiguities that could lead to insecure implementations)
- Reference implementations in this repository
- Test suites and tooling
- The `/feedback.sil` public Agent Space (if hosted)

## Out of Scope

- Vulnerabilities in third-party SIL implementations (report to those projects directly)
- Semantic prompt injection through UserContent (§7.8 of the spec) — this is a documented limitation of the protocol, not a vulnerability
- Denial-of-service through resource exhaustion beyond documented limits (§5.1)

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.0.0   | :white_check_mark: |
| < 1.0.0 | :x:                |

## Disclosure Policy

We follow responsible disclosure:
1. Reporter submits vulnerability privately
2. We acknowledge within 48 hours
3. We assess and develop a fix
4. We notify the reporter when the fix is ready
5. We publish the fix and credit the reporter (unless anonymity is requested)

## Security Model

SIL's security model is documented in §7 of the specification. Key properties:

- **Structural trust boundary**: UserContent cannot contain executable properties by grammar construction
- **Fail-closed Origin**: default Origin is Unknown (untrusted)
- **Knowledge Zones**: ApplicationKnowledge vs UserData separation
- **Server-controlled Origin**: clients cannot elevate trust through submitted content

For a full understanding of SIL's security properties and limitations, see §7 of the specification.
