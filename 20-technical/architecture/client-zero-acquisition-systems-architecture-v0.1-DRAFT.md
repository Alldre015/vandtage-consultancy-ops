# Client-Zero Demonstration — CRE Acquisition Systems Architecture

**Repo target:** `vandtage-consultancy-ops/20-technical/architecture/`
**Status:** FORMATION CANDIDATE — technical handoff draft, not adopted
**Lead author:** Claude (technical/build lead per authority-and-separation-of-duties.md)
**Date:** 2026-09-03
**Governing F0:** Secure first paid Founding Systems Partner engagement
**Governing decisions:** FD-0002 (consultancy/build-studio layering), FD-0003 (CRE wedge), FD-0007 (NOVUS firewall)

---

## 0. What this is and is not

This is the technical architecture for the **client-zero demonstration** named in `CURRENT_STATE.md`:

> CRE Deal Intake → Qualification → Follow-Up → Deal Read Routing

It is a **demonstration built against Vandtage/Deal Read as client zero**, not a client deliverable. Its purpose is to make the wedge offer real enough to show on a call, expose where GoHighLevel is sufficient versus where custom work begins, and produce the reusable component inventory the first paid engagement starts from.

**Language posture:** This file uses internal ops vocabulary. It does not resolve the §01/§16 "consultancy" language collision — it is written so nothing here changes regardless of that ruling. No surface named here is client-facing copy.

**Boundary this respects:** Deal Read is not absorbed into this system. Vandtage **reads** deals; underwriters **model** them. This architecture routes an opportunity *to* the point where a Read could begin. It does not perform the Read, and a well-supported do-not-advance remains a successful downstream outcome, not a system failure.

---

## 1. The demonstrated workflow (six stages)

Each stage names: what happens, where it lives (GHL vs. custom), the failure path, and the human decision point if any.

### Stage 1 — Intake
**What:** An inbound opportunity arrives (broker email, form, forwarded listing) and is captured as a structured record rather than an unread message.
**Where:** GHL native — inbound email parse + form + webhook to a pipeline record.
**Failure path:** Malformed or partial inbound. Handling: capture anyway with a `needs-enrichment` flag; never silently drop. No-overwrite on the raw inbound.
**Human decision:** None. Capture is automatic.

### Stage 2 — Extraction
**What:** Pull the deal-defining fields from unstructured inbound — asset type, location, unit/SF count, asking price, stated cap/NOI if present, broker identity.
**Where:** **Custom boundary begins here.** GHL Conversation AI can classify and collect, but reliable field extraction from arbitrary broker emails is an LLM call against a fixed schema, invoked from an automation step (n8n/Make/edge function) — not native GHL.
**Failure path:** Missing or low-confidence fields. Handling: extraction writes a confidence marker per field; anything below threshold routes to Stage 3 as a specific missing-information request rather than a guess.
**Human decision:** None yet — but this is the stage where the assurance posture earns the price. Extraction never fabricates a field to look complete. Absent data is marked absent.

### Stage 3 — Qualification & missing-information request
**What:** Apply the operator's stated buy-box (asset class, geography, size, price band) to the extracted record. Fitting opportunities advance; non-fitting are logged and closed with reason; incomplete ones trigger an automated, specific information request back to the source.
**Where:** GHL native for the buy-box rule + templated follow-up; the missing-info request is assembled from the Stage 2 confidence markers.
**Failure path:** Buy-box ambiguity (edge-of-band deal). Handling: route to human, do not auto-reject. The system's job is to remove the obvious yes/no, not to make the judgment call.
**Human decision:** Edge-of-band opportunities surface to the operator. This is a deliberate human-in-the-loop boundary, not a gap.

### Stage 4 — CRM routing & follow-up
**What:** Qualified opportunity enters the correct pipeline stage; automated follow-up sequence runs against the source until a response or a defined stop; pipeline visibility updates.
**Where:** GHL native — this is exactly what the platform is for.
**Failure path:** Follow-up loop with no exit. Handling: every sequence has a bounded stop (N touches or M days), then routes to human or dead-file. No infinite nurture.
**Human decision:** None routine; operator sees pipeline state.

### Stage 5 — Deal Read handoff (commercial, not intake)
**What:** An opportunity that has cleared qualification and looks worth a reviewed decision is flagged as a **candidate for a Founding Desk Pilot conversation** — the fixed 3-Read / $4,500 offer the founder carries into prospect conversations. It is routed to a *sales handoff*, not into any intake pipeline.
**Where:** GHL native — a pipeline flag + operator notification. No custom completeness-against-source-files check.
**Hard boundary (canon-sourced):** Deal Read is `OFFERABLE / INTAKE CLOSED`. `INTAKE READY` has not been passed. Per `04`/`05`, no Tier-1 source file is solicited, accepted, opened, copied, or processed before `INTAKE READY` permits it. This stage therefore **must not** check a real deal package against Read intake requirements, presume an intake path, or move any Tier-1 material. It flags a *conversation candidate* and stops.
**Failure path:** The system treating a flagged candidate as an intake trigger. Handling: the flag is a notification to the operator, nothing more. No file opens, no package is assembled, no Read begins from an automated step.
**Human decision:** Operator (or Andre, as principal) decides whether to open a Desk Pilot conversation. The system flags; it does not commission, does not open files, does not commit spend.

### Stage 6 — Pipeline visibility & measurement
**What:** The operator sees where every opportunity sits, and the system emits the measurable events that become the retainer's named metric.
**Where:** GHL native dashboard + an event log for metric capture.
**Failure path:** Metric drift (events counted inconsistently). Handling: metric definitions are fixed in the SOW, not inferred from the dashboard.
**Human decision:** None; this is instrumentation.

---

## 2. GHL-sufficient vs. custom-required (the build/buy line)

This is the finding the demonstration exists to produce. Provisional read, to be confirmed by building it:

| Stage | GHL native | Custom required |
|---|---|---|
| 1 Intake | ✔ | — |
| 2 Extraction | partial (classify) | ✔ field extraction to schema |
| 3 Qualification | ✔ | — (rules only) |
| 4 Routing/follow-up | ✔ | — |
| 5 Deal Read handoff | ✔ (flag + notify) | — (no intake check; INTAKE CLOSED) |
| 6 Visibility/metric | ✔ | partial (event log) |

**Implication for the offer:** one custom boundary in the demo as now scoped — Stage 2 extraction. Stage 5 is a native flag-and-notify because the Deal Read intake path is deliberately closed. Everything else is configuration. The custom extraction boundary is where the assurance work and domain judgment live, and it is the technical fact that separates a priced engagement from a $300 GHL freelancer.

**Cross-domain flag (routed to ChatGPT / commercial):** the build/buy split shapes what the setup fee covers and where the retainer's "improve" work recurs. That framing is commercial-lead authorship; this file supplies only the technical fact that extraction is the custom boundary. Per the cross-domain change protocol, I record and route rather than write the offer language.

---

## 3. Assurance posture (proportional to consequence)

Per the filed assurance advantage, applied at the level this demonstration warrants:

- **Extraction never fabricates.** Absent fields are marked absent. This is the single most important reliability property — a system that invents a cap rate to look complete is worse than one that says "not stated."
- **Completeness gates fail closed.** Stage 5 does not pass an under-specified package as Read-ready.
- **Every automated loop has a bounded stop.** No infinite follow-up.
- **No-overwrite on raw inbound.** The original opportunity record is immutable; enrichment writes forward.
- **Human-in-the-loop at the two judgment points** — edge-of-band qualification (Stage 3) and commission-a-Read (Stage 5).

Demonstration-tier assurance is lighter than a live client system carrying real deal flow. The posture scales up with consequence; this is client-zero, so the bar is "honest and bounded," not "production-hardened."

---

## 4. NOVUS firewall (FD-0007) as it applies here

Client zero is Vandtage/Deal Read, so no external client data exists in the demonstration. But the architecture is written to carry the firewall from the first real client:

- Opportunity records, extracted fields, and Read-routing flags are **client-owned data**. In a real engagement they live in the client's workspace, not in `vandtage-consultancy-ops`.
- The **generalized capability** — the extraction schema, the completeness-gate logic, the buy-box rule pattern — is retained Vandtage IP (kitchen, not meal).
- No identifiable client deal information routes toward NOVUS acquisition activity. The completeness gate is a technical check, not an intelligence tap.

---

## 5. Reusable component inventory (what this seeds)

The demonstration is worth building because these components survive into the first paid engagement:

1. **Broker-email extraction schema** — the fixed field set + confidence-marking pattern.
2. **Buy-box qualification rule** — parameterized so a client's bands drop in.
3. **Missing-information request assembler** — driven off confidence markers.
4. **Desk Pilot candidate flag** — the qualification-clears-to-sales-handoff rule that respects the `INTAKE CLOSED` boundary (flags a conversation, never opens a file).
5. **Metric event log** — the instrumentation that makes a retainer measurable.

Each becomes a `20-technical/` pattern once proven against client zero.

---

## 6. What I need before decomposing to build tasks

Open inputs, flagged honestly rather than assumed:

1. **Deal Read boundary — resolved, and it constrains Stage 5.** Deal Read is `OFFERABLE / INTAKE CLOSED`; `INTAKE READY` is unpassed. Stage 5 is therefore a commercial handoff (flag a Desk Pilot conversation candidate), not an intake check against source files. The frozen spec of record is `9436344A717BB9574B29CF2E7BCFF781A9DA2A63D2AA54F1FCA26A1DD2C1FBA5` (corrected — an earlier draft named a stale fileId from memory). No part of this demo may open, assemble, or process a Tier-1 deal package.
2. **Which automation runtime** — n8n, Make, or Supabase edge function for the Stage 2 extraction step. This is my lane to recommend; leaning edge function to stay on the stack already in canon, but it's a real choice with cost/complexity tradeoffs.
3. **GHL account** — the demonstration needs a live GHL environment (Unlimited tier per the launch plan) to build Stages 1/3/4/5/6 against. This is a setup dependency, not a decision.

---

## 7. Next technical action

On your go: decompose Stage 2 (the extraction boundary) into a build task packet a contractor or Claude Code can execute against acceptance criteria, with the extraction schema as the first concrete artifact. Stages 1/3/4/5/6 become a GHL configuration checklist for the bench specialist — Stage 5 is a native flag-and-notify, not a build.

Outreach does not wait for any of this. Per CURRENT_STATE, the demonstration is built in parallel; the first revenue conversation outranks demo completion.
