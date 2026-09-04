# Founder Decision Ledger

**Authority:** Andre  
**Mutation rule:** Append-only. Existing entries are not rewritten to erase history. A later decision supersedes an earlier decision by explicit reference.

---

## FD-0001 — Consultancy Activation

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Activate the Vandtage AI consultancy as a revenue-first commercial workstream operating alongside Deal Read Desk and the Vandtage Build Studio.

**Implication:** The consultancy moves from strategy formation into active market execution.

---

## FD-0002 — Consultancy / Build Studio Architecture

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Consultancy and Build Studio operate as complementary layers of one Vandtage systems business. Consultancy acts as a cash-generating front door; custom software and implementation escalate into the Build Studio where required.

---

## FD-0003 — Market Wedges

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Maintain both CRE Acquisition Systems and non-CRE Revenue Systems as active market wedges.

**Initial allocation:** Approximately 70% CRE / 30% non-CRE founder attention.

**Qualification:** Allocation is provisional and may be changed by Founder ruling when market evidence warrants.

---

## FD-0004 — Founding Offer Pricing Test

**Date:** 2026-09-03  
**Status:** ACTIVE TEST  
**Decision:** Test the Founding Systems Partner offer at:

- $3,500–$5,000 setup
- $3,500/month managed engagement
- 90-day initial term

Alternative commercial structure:
- $5,000/month × 3 months

**Qualification:** These are test prices, not permanent pricing canon.

---

## FD-0005 — W-2 Exit Economic Trigger

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Current planning continues to use a sustained **$10,000/month** revenue trigger as the W-2 replacement line unless Founder changes it.

---

## FD-0006 — Contractor-First Delivery

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Build a contractor bench before speculative employee payroll. Client revenue should activate delivery capacity.

---

## FD-0007 — NOVUS / Client Confidentiality Firewall

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Vandtage may retain generalized know-how and capability generated through lawful client work, but client-confidential information, identifiable deal information, credentials, proprietary data, or client acquisition intelligence may not be used for NOVUS proprietary acquisition activity.

---

## FD-0008 — Shared Operational Repository

**Date:** 2026-09-03  
**Status:** ACTIVE  
**Decision:** Establish `vandtage-consultancy-ops` as the shared operational control plane for Founder decisions, commercial operating state, technical architecture, and cross-seat continuity.

**Operating rule:** Shared visibility, bounded authorship. No seat may silently mutate another seat's governed domain.

---

## FD-0009 — Client-Zero Architecture Candidate: Bounded Repair Before CRP Dispatch

**Date:** 2026-09-04  
**Status:** ACTIVE  
**Ruling reference:** FR-1  
**Decision:** **Do not dispatch the current client-zero architecture candidate for consolidated review yet.** The filed candidate (`20-technical/architecture/client-zero-acquisition-systems-architecture-v0.1-DRAFT.md`, `867E82F5EF628A1B176D57054898C008058AA6C48ACF56BE719E7897EBF30ACD` · 11,632 B · 140 ln) contains known superseded-draft residue concerning Stage 5. Route it to the technical forming role for a **narrowly bounded, source-grounded repair** first.

**Repair scope — limited to eliminating contradictions created by the retired Stage-5 completeness-gate design, including at minimum:**

- the §3 statement that Stage 5 prevents an under-specified package from becoming Read-ready;
- the §3 description of the Stage-5 human judgment as "commission-a-Read";
- the §4 reference to "completeness-gate logic" as reusable capability, where that language refers to the retired Stage-5 mechanism;
- the §4 statement describing "the completeness gate" as a technical check;
- any directly equivalent stale Stage-5 residue found during the same bounded repair.

**Governing Stage-5 design:**

> qualification clears → commercial flag / notification → human decision whether to open a Desk Pilot conversation → stop.

No Tier-1 intake, package completeness gate, Read commissioning, or automated Read initiation occurs at Stage 5 while Deal Read remains `OFFERABLE / INTAKE CLOSED`.

**Do not broaden the repair into architectural redesign.**

**After repair, in order:** (1) establish the new artifact identity/digest; (2) reconcile any dependent state made stale by the byte change; (3) rerun the applicable CRP Step 0 treatment; (4) return the repaired candidate for Founder authorization to dispatch.

**Rationale of record:** the known defect is removed before consolidated review so review capacity is spent discovering unknown defects rather than rediscovering a defect already measured.

---

## FD-0010 — No Automation Runtime Selected

**Date:** 2026-09-04  
**Status:** ACTIVE  
**Ruling reference:** FR-5  
**Decision:** **No runtime is selected.** Do not choose Supabase Edge Functions merely because prior documentation suggests Vandtage already operates that runtime.

**Verification requirement:** the technical seat must **verify the live deployment surface** before existing-runtime reuse is used as a decision premise. Recorded trigger: `docs/ai-context/edge-functions.md` in `Alldre015/vandtage` names eight built functions, while no `supabase/functions/` tree exists at any commit in that repository's history — so the reuse premise rests on a deployment surface no seat has yet confirmed.

**The next runtime decision compares the actually available options** — including Supabase Edge Functions, n8n, Make, or another justified mechanism — **against:** verified current infrastructure · incremental recurring cost · implementation complexity · observability · assurance/testability · data and control requirements · expected reuse across client work.

**Authority shape:** this is a **technical recommendation for Founder acceptance**, not a pre-decided platform choice.

---

## FD-0011 — Precedence of This Ledger (cross-reference, not a local decision)

**Date:** 2026-09-04  
**Status:** ACTIVE  
**Ruling reference:** FR-2 · FR-3 · FR-4  
**Decision:** Recorded here **by cross-reference only**, because these rulings govern holdco-wide and this ledger may not restate or independently reinterpret them.

**Governing record:** `Alldre015/vandtage` → `docs/ai-context/04-decision-assumption-ledger.md` v1.57, section **"2026-09-04 — Founder-decision surface precedence, seat write-authority retention, and scoped v5 staleness (three founder rulings)."**

**What FR-2 establishes about this file:** the holdco Decision & Assumption Ledger (`04`) is the **higher governing decision surface**. This file is a **local operating decision ledger for the Vandtage consultancy** and does **not** create a parallel source of Founder authority equal to or above `04`. Precedence, highest first: **live Founder ruling → applicable holdco canonical governance / `04` → this ledger → subordinate operating artifacts → conversation narration.**

**A decision may remain local** when its consequence is confined to the consultancy operating surface. **A decision must be surfaced for holdco reconciliation** when it materially affects: cross-holdco governance · NOVUS or another entity · canonical authority · capital or ownership policy · shared infrastructure · an existing holdco decision · a dependency governed outside the consultancy.

**Where the two ledgers conflict, this ledger may not silently supersede `04`.** Cross-references are used rather than duplication or independent reinterpretation — this entry is written to that rule and deliberately carries no restatement of FR-3 or FR-4 beyond their existence and location.
