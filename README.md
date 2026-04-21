# sdpf.github.io
# SDPF — Software Development Prompting Framework

**Version 1.3.1 · Hamza Abdullah · 2026**

> *Problem first. Specification second. Facts before execution. Verification always.*

---

## The Problem

The AI industry has systematically misidentified the primary failure mode of AI-assisted software development. The failure is attributed to the model — it hallucinated, it misunderstood, it chose the wrong approach.

The correct diagnosis is different.

When a specification is incomplete or contains unverified technical facts, an AI model fills the gaps with its best guess. The output looks plausible. It is not what was intended. What the industry calls hallucination is, in the overwhelming majority of practical cases, **specification-induced speculation**.

Better models make better guesses. They are still guessing.

**The correct intervention is not a better model. It is a complete specification — one where there is nothing to guess.**

---

## What SDPF Is

SDPF is a formal specification language that makes complete specifications possible. It defines the vocabulary, grammar, semantics, and enforcement mechanisms that eliminate ambiguity at the source.

A specification written in SDPF gives an AI model nothing to speculate about. The output is not a better guess. It is a **mechanical execution of a verified contract**.

**The model is not the variable. The specification is the constant. When the constant is correct, every capable model gives a correct answer.**

---

## How It Works

The gate sequence is enforced in code — not policy. These are `RuntimeError` exceptions that cannot be bypassed:

```python
def generate_tests(self):
    if self.state.stage not in {Stage.SPEC_LOCKED, Stage.TESTS_GENERATED}:
        raise RuntimeError("Tests can only be generated after the specification is locked.")

def generate_implementation(self, module_name: str = "sdpf_module"):
    if self.state.stage != Stage.TESTS_LOCKED:
        raise RuntimeError("Implementation can only be generated after tests are locked.")

def verify(self):
    if self.state.stage != Stage.CODE_GENERATED:
        raise RuntimeError("Verification is only available after implementation generation.")
```

There is no override. There is no skip option. There is no admin mode. The only way to proceed is to complete the upstream steps. Engineering intent becomes **structurally impossible to ignore** — not merely discouraged.

---

## The Three Core Principles

| # | Principle | Definition |
|---|-----------|------------|
| I | **Specification First** | No implementation begins until the contract is complete, locked, and TVG-verified |
| II | **Facts Before Execution** | Every asserted technical fact — versions, endpoints, paths, variables — is verified against live output before implementation begins |
| III | **Verification Always** | No release without a complete Verification Closure Record. All 11 structural invariant checks must pass. CLOSURE STATUS must be COMPLETE |

---

## The Lifecycle

```
PHASE 0 ──────────────────────────────────────────────────────────────
  P0-1  Observe      Measure current state against desired state
  P0-2  Define       Write and validate the problem statement (4 tests)
  P0-3  Own          Confirm the problem owner

  ↓  Gate: Validated problem statement required to enter Phase 1

PHASE 1 ──────────────────────────────────────────────────────────────
  S-1   TSP_DRAFT         Write specification from problem statement
  S-2   SPEC_LOCKED       TVG verified. Conflicts resolved. REQ-IDs assigned.
  S-3   TESTS_GENERATED   Tests derived from locked specification
  S-4   TESTS_LOCKED      Tests frozen. Implementation gate opens.
  S-5   CODE_GENERATED    Implementation derived from locked tests
  S-6   VERIFIED          All 11 structural invariant checks pass
  S-7   EVIDENCED         Signed evidence package exported and archived
```

Every stage gate is enforced by the tool. Out-of-sequence actions produce blocking errors.

---

## The Technical Verification Gate (TVG)

The TVG is Core Principle II and a mandatory section of every SDPF specification.

Before any code is generated, every technical fact stated in the specification — tool versions, API endpoints, file paths, environment variables — must be verified by running a command in the actual target environment. If the output does not match, **work stops**.

```
Asset            Asserted Value    Verification Command        Pass Condition
──────────────   ──────────────    ─────────────────────────   ──────────────
Python           3.13.1            python --version            Python 3.13.1
requests         2.32.3            pip show requests           Version: 2.32.3
ICPSR_SECRET     SET               python -c "import os; ..."  SET
State dir        writable          python -c "pathlib..."      writable
```

The TVG is where the framework meets reality. Without it, a perfectly structured specification can produce broken software — because the specification's assertions about the world were never checked against the world.

---

## The Bounded Stochasticity Theorem

SDPF resolves an apparent contradiction: the methodology is deterministic, but the executor — an AI model — is stochastic.

**THEOREM:** Let `S` be a complete SDPF specification defining a set of structural invariants `I`. Let `G` be a generative AI output produced from `S`. `G` satisfies `S` if and only if `G` satisfies all invariants in `I`. The stochastic variation in `G` is bounded by `I` — any variation that satisfies `I` is a conforming realisation of `S`.

AI output does not need to be deterministic to be reliable. It needs to be **structurally invariant**. Two implementations that differ entirely in surface form but satisfy the same invariants are both conforming realisations of the specification.

This theorem is the reason SDPF is not merely a methodology. It is the formal property that makes the discipline defensible to certification bodies, regulators, and independent implementers.

---

## Phase 0 — Problem Definition

No specification may be written until Phase 0 produces a validated problem statement. This is not a recommendation. It is a blocking gate.

A valid problem statement has four components:

| Component | Must Be | Must Not Be |
|-----------|---------|-------------|
| **Current State** | Observable and measurable. Present tense. | An assumption, opinion, or cause |
| **Desired State** | Specific and testable | Vague. "Better" without a measure |
| **Gap** | Quantified — a number, frequency, percentage, or time | Qualitative only |
| **Impact** | Cost, risk, time, or compliance consequence | A technical description |

**Four validation tests — all must pass:**

- **T-1 Observable** — the problem can be seen or measured in the real world
- **T-2 Bounded** — the problem has a clear start and end
- **T-3 Cause-Free** — no language implying why the problem exists
- **T-4 Solution-Free** — no language implying how it will be solved

---

## Requirement Priority System

Every requirement must begin with a priority tag:

```
[CRITICAL]   System fails or does not close the problem gap without this
[REQUIRED]   System is incomplete without this. Yields to CRITICAL in conflict.
[OPTIONAL]   Enhancement only. Yields to CRITICAL and REQUIRED.
```

Conflicts are resolved automatically by strictness priority. The only conflict requiring practitioner action is `CRITICAL` vs `CRITICAL` — and only at specification time, never at execution time.

---

## The 17 SDPF Styles

Style selection is the first decision in Phase 1. It is made before any requirement is written.

| ID | Style | Best For |
|----|-------|---------|
| 1 | Technical Spec | Deterministic systems with fully definable I/O |
| 2 | Exploratory | Requirements that must emerge through exploration |
| 3 | UX-Centered | User interfaces and interactive experiences |
| 4 | Creative Scaffolding | Generative and creative systems |
| 5 | Iterative Refinement | Frequently changing requirements |
| 6 | System Architecture | Distributed multi-service systems |
| 7 | Test-Driven | Libraries, APIs, critical infrastructure |
| 8 | Constraint-Based | Hardware constraints, real-time performance |
| 9 | Maintenance / Refactor | Improving or preserving existing working code |
| 10 | Compliance-Driven | Healthcare, finance, government, regulated domains |
| 11 | Data Pipeline | ETL, streaming, data transformation |
| 12 | Sovereign Structural Ledger | Immutable audit ledgers, chain-of-custody |
| 13 | Event-Driven | Event sourcing, message queues, reactive systems |
| **14** | **Dynamic Criticality** | **Safety-critical systems with runtime state-dependent escalation** |
| 15 | Cross-System Verification | Formal verification, invariant checking |
| 16 | Multi-Agent Orchestration | Coordinated AI agent systems |
| 17 | Adaptive Learning | Systems that learn and modify behaviour over time |

---

## Evidence Package

Every verified SDPF run produces a signed evidence package — a tamper-evident record containing:

- Validated problem statement from Phase 0
- Complete locked specification with all REQ-IDs
- Full test suite with REQ-ID traces
- SHA-256 hash of the implementation
- Verification Closure Record (CLOSURE STATUS: COMPLETE)
- Traceability matrix — REQ-ID → TEST-ID → implementation
- HMAC-SHA256 provenance signature

Any modification after signing invalidates the signature. The evidence package is not documentation produced after the fact. It is **proof** that the three core principles were satisfied.

---

## Conformance Levels

| Level | Name | Key Requirement |
|-------|------|----------------|
| 1 | Specification Conformance | All sections present. TVG completed. No CRITICAL-CRITICAL conflicts. |
| 2 | Tool Conformance | Gate sequence enforced in code. Phase 0 gate enforced. Evidence package produced and signed. |
| 3 | Evidence Conformance | All required fields present. Stage = VERIFIED. Valid provenance signature. CLOSURE STATUS = COMPLETE. |
| 4 | Process Conformance | Phase 0 completed before every project. Level 2 tool used. Level 3 evidence for every release. |

---

## Reference Implementation

The **Industrial Controller PSR** is the complete reference implementation demonstrating the full SDPF lifecycle.

- **Domain:** Mission-critical industrial process control
- **Style:** 14 — Dynamic Criticality Extension
- **Requirements:** 27 (15 CRITICAL, 10 REQUIRED, 2 OPTIONAL)
- **Tests:** 56
- **Result:** 56/56 passing
- **CLOSURE STATUS:** COMPLETE
- **Environment:** Python 3.13.1 · Windows 11 · conda in_control

The TVG caught a real error during development: every draft specification asserted Python 3.11 on Linux. The actual environment was Python 3.13.1 on Windows 11. The TVG stopped this before a single line of implementation was written — at zero cost.

---

## Five Mechanisms That Make Drift Structurally Impossible

| # | Mechanism | What It Prevents |
|---|-----------|-----------------|
| 1 | **Specification locked before anything else exists** | Intent cannot be skipped — it is the prerequisite for everything |
| 2 | **REQ-IDs permanently identify every requirement** | Untraced code is immediately visible |
| 3 | **Gate sequence enforced in code as hard errors** | Out-of-sequence actions are impossible, not merely discouraged |
| 4 | **Eleven verification checks block release** | Incomplete runs cannot produce passing evidence |
| 5 | **HMAC-SHA256 signed evidence package** | Retroactive alteration of the record is detectable |

No single mechanism is sufficient alone. All five together close every path by which engineering intent can be abandoned.

---

## High-Assurance Applications

PSR + SDPF maps directly to the requirements of major high-assurance regulatory frameworks:

| Standard | Domain | SDPF Satisfies |
|----------|--------|---------------|
| DO-178C | Avionics | Requirements traceability, independence of testing, verification evidence, configuration management |
| ISO 26262 | Automotive | Hazard analysis, safety goal traceability, ASIL allocation, verification coverage |
| IEC 62304 | Medical devices | Software lifecycle, requirements traceability, risk management, Class C compliance |
| IEC 61508 | Functional safety | SIL determination, requirements specification, verification, functional safety management |
| NERC CIP | Power grids | Configuration change management, security controls documentation |
| NASA NPR 7150.2 | Spacecraft | Software classification, requirements traceability, IV&V evidence |
| FDA 21 CFR Part 11 | Medical records | Electronic record integrity, audit trail, access controls |

---

## Repository Contents

```
SDPF_Language_Specification_v1_3_1.pdf   Authoritative language specification
SDPF_Styles_Playbook.pdf                 All 17 styles with application guides
SDPF_Engineering_Intent.pdf              Five mechanisms of structural enforcement
SDPF_Architect_Thinking.pdf             Master practitioner mental model
SDPF_Problem_Definition.pdf             Phase 0 problem identification procedure
Bounded_Stochasticity_Theorem.pdf        Formal theorem and proof
Technical_Verification_Gate.pdf         TVG design and rationale
SDPF_A_New_Science.pdf                  Foundational positioning
```

---

## How to Cite

```
Abdullah, H. (2026). SDPF Language Specification, Version 1.3.1.
Software Development Prompting Framework.
```

---

## Licence

MIT License — Copyright 2026 Hamza Abdullah

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

---

*Problem first. Specification second. Facts before execution. Verification always.*