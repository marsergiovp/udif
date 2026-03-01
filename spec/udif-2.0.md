# UDIF 2.0 Specification

**Status:** Draft  
**Version:** 2.0.0  
**Invented by:** Angela Benton  
**Original standard:** Streamlytics, Inc. (2018)  
**License:** Apache 2.0  
**Schema:** `/spec/schema/udif.schema.json`

---

## Overview

UDIF (Universal Data Interchange Format) is an open standard for 
portable, sovereign data. Version 2.0 extends the original standard 
into the AI interaction layer.

A UDIF file is a structured JSON document that represents a person's 
data in a format that is:

- **Platform-agnostic** — readable by any system without permission
- **Self-contained** — no platform dependency to access or interpret
- **Consent-forward** — consent type is captured in the format itself
- **Verifiable** — provenance chain proves origin and integrity
- **Human-centered** — captures emotional, energetic, and values context alongside behavioral data
- **Extensible** — modular structure supports any data type

---

## Required Fields

Every valid UDIF 2.0 file must contain:

- `udif` — version identifier
- `meta` — source, session, timestamp, and consent
- `platform` — origin platform details
- `data_event` — the interaction or data being captured

All other modules are optional but recommended for richer portability.

---

## Modules

### `udif`
Version string. Always `"2.0"`. Identifies the file as UDIF and 
specifies the specification version.

---

### `meta`
Session-level metadata about the data capture.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| source | string | yes | Platform or system generating the file |
| generator | string | no | Specific model or tool used |
| session_id | string | yes | Unique session identifier |
| timestamp | datetime | yes | ISO 8601 timestamp of capture |
| user_id | string | no | User identifier on the source platform |
| consent_granted | boolean | yes | Whether user has consented to this capture |
| consent_type | string | no | `explicit`, `contextual`, or `implied` |
| data_type | string | no | Category of data being captured |

---

### `identity`
Information about the data subject. All fields are optional to 
protect privacy while enabling personalization.

| Field | Type | Description |
|-------|------|-------------|
| alias | string | Pseudonym or display name |
| public_key | string | Cryptographic public key for verification |
| persona_tags | array | Self-described identity tags |
| archetype | string | Self-described archetype or role |
| language | string | Preferred language code |
| timezone | string | IANA timezone string |

---

### `context`
Environmental and state context at the time of the data event. 
Captures the human conditions surrounding the data, not just 
the data itself.

| Field | Type | Description |
|-------|------|-------------|
| location | object | Latitude, longitude, and optional geo label |
| device | object | Device type and operating system |
| emotional_state | array | Self-reported or inferred emotional states |
| presence_level | string | `low`, `medium`, or `high` |
| energy_score | integer | 1–10 self-reported energy level |

---

### `platform`
Details about the source platform and export format.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | string | yes | Platform name |
| data_format | string | yes | Format of the original export |
| source_type | string | yes | Type of data source |
| export_date | datetime | yes | When the data was exported |
| raw_schema_reference | string | no | URL to the platform's original schema |
| session_reference_id | string | no | Platform's own session identifier |

Supported platform values: `OpenAI`, `Google`, `Anthropic`, 
`Perplexity`, `Custom`

Supported source types: `chat_log`, `file`, `search`, `audio`, 
`image`, `multimodal`

---

### `data_event`
The core interaction or data being captured. For AI interactions 
this includes the full message thread.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| type | string | yes | Type of event |
| service | string | yes | Service within the platform |
| tags | array | no | Descriptive tags |
| content_title | string | no | Human-readable title for the event |
| duration | integer | no | Duration in seconds |
| value_perceived | integer | no | User-perceived value, 1–10 |
| is_shareable | boolean | no | Whether user consents to sharing |
| messages | array | no | Individual messages in the interaction |
| shared_links | array | no | Links shared during the interaction |

**Message object:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| role | string | yes | `user` or `assistant` |
| content | string | yes | Message content |
| timestamp | datetime | yes | ISO 8601 timestamp |

---

### `values`
Explicit user-defined permissions and preferences governing how 
their data can be used.

| Field | Type | Description |
|-------|------|-------------|
| data_use_permissions | array | Permitted uses of this data |
| sharing_boundaries | array | Explicit limits on sharing |
| preferred_exchanges | array | Value exchanges the user accepts |
| core_values | array | User's stated core values |

---

### `frequency`
Captures the human signal beneath the data — the energetic and 
creative context that purely behavioral formats miss. This module 
is unique to UDIF.

| Field | Type | Description |
|-------|------|-------------|
| intended_impact | string | What the user intended to achieve |
| creative_source | string | Origin of the creative or intellectual energy |
| self_expression_level | string | `low`, `medium`, or `high` |
| authenticity_score | integer | 1–10 self-reported authenticity |
| energetic_signature | string | Unique hash representing this session's energy pattern |

---

### `provenance`
Cryptographic metadata establishing the origin, authenticity, 
and integrity of the file. The `chain` array is what makes UDIF 
verifiable — not just portable.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| created_at | datetime | yes | When this UDIF file was created |
| updated_at | datetime | no | When this file was last modified |
| source | string | yes | Originating platform |
| hash | string | no | SHA-256 hash of the data_event object |
| signature | string | no | Cryptographic signature proving authorship |
| chain | array | no | Auditable history of platform transfers |

**Chain object:**

| Field | Type | Description |
|-------|------|-------------|
| platform | string | Platform name at this point in the chain |
| exported_at | datetime | When data left this platform |
| hash | string | Hash of data at export, enabling tamper detection |

The chain creates an auditable trail from origin through every 
platform the data has touched. Each hash allows any system to 
verify the data hasn't been altered in transit.

---

### `raw_payload`
The original unmodified data from the source platform, preserved 
alongside the standardized UDIF structure. Ensures nothing is lost 
in translation.

### `raw_payload_ref`
A URI reference to the raw payload if stored externally rather 
than inline.

---

## Versioning

UDIF follows semantic versioning. The `udif` field in every file 
identifies the version of the spec it conforms to. Breaking changes 
increment the major version.

---

## The Frequency Module

The `frequency` module deserves additional context because it has 
no equivalent in any other data standard.

Most data formats capture what happened. UDIF captures what it 
meant to the person it happened to. The frequency module was 
designed on the premise that human data has an energetic quality 
that behavioral data alone cannot represent — the intention behind 
an interaction, the creative state it came from, the authenticity 
of the expression.

This is not metadata in the traditional sense. It is human signal.

---

## Contributing

See `CONTRIBUTING.md` for guidelines on contributing to the 
specification.

---

## References

- Patent: US20220300636A1 — *System and Method for Standardizing Data*
- Inventor: [Angela Benton](https://angelabenton.com)
- Protocol: [Heirloom](https://yourheirloom.ai)
