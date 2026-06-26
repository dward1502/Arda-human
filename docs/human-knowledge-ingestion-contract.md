---
soterion:
  sigil: "SCROLL"
  glyph: "📜"
  code_point: "U+1F4DC"
  role: "documentation"
  owner: "HADES"
  status: "active"
  last_reviewed: "2026-05-21"
---

> 🜏 Soterion: 📜 documentation | owner: HADES | status: active | reviewed: 2026-05-21

# Human Knowledge Ingestion Contract

Contract: annunimas.human_knowledge.v1
Status: active-draft
Owner: ATHENA / MNEMOSYNE / HADES / PROMETHEUS
Scope: `human/**`

## Purpose

This contract prevents raw human notes, generated summaries, old plans, and canonical decisions from being treated as the same kind of truth.

`/human` is private by default. Ingestion must classify information before agents rely on it.

## Directory Semantics

Recommended target structure:

```text
human/
  inbox/          raw drops; untrusted until classified
  sources/        immutable imports; preserve original source material
  working/        drafts, thoughts, incomplete notes
  canonical/      promoted human truth; agents may rely on this
  decisions/      explicit human/sovereign decisions and overrides
  knowledge/      processed knowledge articles
  summaries/      generated summaries and system notes
  plans/          human-facing plans; not automatically canonical
  media/          attachments
  templates/      reusable note templates
  triggers/       temporal/action trigger rules
  archive/        historical or superseded material
```

Existing directories such as `human/library`, `human/summaries`, and `human/plans` are valid legacy locations. They must be classified before migration. Do not bulk-move them without a HADES lifecycle review.

## Authority Levels

Use the lowest authority compatible with evidence.

- `raw`: unprocessed drop, source export, personal note, transcript, scratchpad
- `agent_generated`: generated summary, audit note, derived artifact
- `human`: human-authored or human-approved working truth
- `governance`: explicit policy, covenant, decision, override
- `runtime`: live state or service-derived evidence

Authority does not equal status. A human-authored draft can still be `status: working`.

## Status Values

- `inbox`: received but not processed
- `working`: useful but not canonical
- `candidate`: proposed for promotion or action
- `canonical`: approved truth; safe for agent reliance
- `superseded`: replaced by newer authority
- `archived`: retained for history; not active truth
- `quarantine`: sensitive, contradictory, malformed, or unsafe to auto-use

## Required Frontmatter

New processed Markdown files should include:

```yaml
---
annunimas_contract: human_knowledge.v1
title: ""
status: inbox | working | candidate | canonical | superseded | archived | quarantine
source_type: note | plan | decision | source | summary | research | transcript | media | trigger
authority: raw | agent_generated | human | governance | runtime
owner: human | athena | mnemosyne | prometheus | hades | oracle
created: YYYY-MM-DD
updated: YYYY-MM-DD
supersedes: []
superseded_by: []
affected_agents: []
affected_paths: []
privacy: private | sensitive | exportable
review_required: true
confidence: low | medium | high
sigils: []
---
```

## ATHENA Responsibilities

ATHENA may:

1. Scan files.
2. Classify source type, authority, and status.
3. Extract candidate metadata.
4. Summarize content.
5. Identify affected agents and paths.
6. Detect contradictions against higher-authority sources.
7. Emit candidate records for review.

ATHENA must not:

- silently promote raw content to canonical
- delete or move human files
- overwrite human-authored source material without explicit approval
- treat old plans as active merely because they exist

## MNEMOSYNE Responsibilities

MNEMOSYNE may:

- preserve processed memories and recall indexes
- provide retrieval across canonical and working knowledge
- retain provenance links back to source files

MNEMOSYNE must distinguish generated recall from canonical truth.

## HADES Responsibilities

HADES may:

- mark stale, duplicate, superseded, orphaned, or archive-candidate files
- recommend lifecycle transitions
- emit audit evidence

HADES must not delete human knowledge by default. Disposal is not deletion. Deletion requires explicit policy and proof.

## PROMETHEUS / CEO Responsibilities

PROMETHEUS may:

- convert approved candidates into tasks
- schedule bounded review work
- require human review for high-risk knowledge changes

PROMETHEUS must enforce action classes from the autonomy governance contract.

## Conflict Handling

When two files conflict:

1. Prefer higher authority source.
2. Prefer newer canonical source over older canonical source only if supersession is explicit.
3. Mark unresolved conflict as `candidate` or `quarantine`.
4. Emit a review task instead of guessing.

## Minimum Ingestion Output Record

ATHENA should emit records shaped like:

```json
{
  "contract": "annunimas.human_ingestion_result.v1",
  "source_path": "human/inbox/example.md",
  "content_hash": "sha256:...",
  "detected_status": "working",
  "detected_authority": "raw",
  "source_type": "note",
  "affected_agents": ["athena"],
  "affected_paths": ["human/"],
  "summary": "...",
  "conflicts": [],
  "recommendation": "retain-working",
  "review_required": true
}
```
