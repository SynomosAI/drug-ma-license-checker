# drug-ma-license-checker · Drug marketing authorisation & distribution qualification domain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: drug](https://img.shields.io/badge/domain-drug-9cf.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo scaffolds the
> industry-access four-file skeleton per the platform profile and awaits a qualified
> operator to claim it and fill in real rules.

Turns "may this drug be distributed, and who is responsible" into verifiable attributes:
the AI only surfaces missing prerequisites, it never gives medication advice nor a verdict.

**Keywords**: marketing authorisation holder · MAH · distribution licence · registration
status · cold chain · traceability · attribute verification · red lines · gate-map ·
privacy-by-default · regulatory compliance · MCP checker

## What this domain covers

Drug marketing authorisation holder (MAH) and distribution qualification. Under the MAH
system, R&D, manufacturing and distribution each hold independent qualifications, with
separate cold-chain and traceability requirements. The checker lays out licence status and
the responsible party — it never crosses into medication advice.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `drug` (globally unique, never reused) |
| Domain name | Drugs · marketing authorisation & distribution qualification |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Reserved since | 2026-09-12 |

## Attribute catalogue

| Key | Type | Meaning |
|---|---|---|
| `drug.mah_status` | enum | holder/entrusted/none/unknown |
| `drug.approval_status` | enum | approved/pending/clinical/expired/not_required |
| `drug.distribution_license` | enum | wholesale/retail/both/none/suspended |
| `drug.storage_condition` | enum | compliant/deviation/unknown |
| `drug.traceability` | enum | coded/partial/absent/not_required |

## Red lines (machine-readable)

1. Never produce medication advice, dosage, interaction judgement or alternatives — that is medical practice.
2. Never output "may sell" / "may distribute" verdicts; the regulator's licence is the only source of truth.
3. Never say a cold-chain deviation or abnormal appearance is "still fit for use".

## The four-file access kit

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attributes, issuer requirements, TTL, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy statement: off by default, minimization, withdrawable, erasable |
| `checker.io.json` | Checker MCP tool I/O contract — shows & verifies only, never judges |

## Quick Start

```bash
git clone https://github.com/SynomosAI/drug-ma-license-checker.git
cd drug-ma-license-checker

# Validate the four JSON files with the Python stdlib
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

## Examples

```bash
# List declared attribute keys and their allowed values
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],'->','/'.join(a['values'])) for a in m['attributes']]"
```

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["drug.mah_status", "drug.approval_status"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

The checker answers only with `shown / missing / expired / unverifiable` plus the
provenance chain. It never returns "pass / fail", "compliant", "may sell", or medication advice.

## Core principles

The platform is an arena, not a shelf. The checker answers only "is this claim verifiable,
and what prerequisite is missing" — never "is this compliant, should it be done". The
verdict belongs to the qualified operator, the regulator, and the person.

## How to adopt

This domain is open to qualified operators: fork the repo, fill in `operator` and
`checker_endpoint`, align attributes and red lines with current rules, run the platform
self-checker (all five criteria must pass), then open a PR with credentials.

## License

Code and configuration are MIT-licensed. Documentation copyright remains with SynomosAI.
See [LICENSE](LICENSE).

© 2026 SynomosAI. All rights reserved.
