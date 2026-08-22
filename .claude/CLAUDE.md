# Academic Research Skills

A suite of Claude Code skills for rigorous academic research, paper writing, peer review, and pipeline orchestration.

## Skills Overview

| Skill | Purpose | Key Modes |
|-------|---------|-----------|
| `deep-research` v2.12.1 | 13-agent research team | full, quick, socratic, review, lit-review, three-way-scan, fact-check, systematic-review |
| `academic-paper` v3.3.1 | 12-agent paper writing | full, plan, outline-only, revision, revision-coach, abstract-only, lit-review, format-convert, citation-check, disclosure, rebuttal-audit |
| `academic-paper-reviewer` v1.11.1 | Multi-perspective paper review (5 reviewers + optional cross-model DA critique) | full, re-review, quick, methodology-focus, guided, calibration |
| `academic-pipeline` v3.20.1 | Full pipeline orchestrator | (coordinates all above) |

## v3.20.1 Key Additions (contract honesty + bounded evaluation substrates)

- **Integrity and review claims match their evidence.** Claim coverage is explicitly bounded to registered or lexically detected populations; semantic completeness remains unknown. Revision claim-strength changes require a byte-bound, per-finding author disposition. Reviewer outputs use categorical criterion judgements, remain `NOT_CALIBRATED` in live packages, and carry typed panel provenance instead of a binary independence claim.
- **Read attestations and Socratic exits fail visibly.** New read marks require an explicit scope and are resolved from validated, append-safe ledgers; malformed or absent attestations cannot become positive coverage. A user may visibly exit non-generating Socratic RQ mode without the system silently generating candidates.
- **Evaluation substrates stay bounded and unmeasured.** The claim-standing probe gains closed contracts, discovery adapters, an offline stance runner/renderer, and synthetic seed assets; the ideation-diversity bundle gains a closed first-round assignment gate. No live stance provider, effectiveness result, semantic-completeness guarantee, independent-error proof, or scientific-outcome claim ships with this patch.
- **Future structure remains opt-in.** The v3.20→v3.22 roadmap stages domain profiles, inquiry branches, alternative registers, capability evidence, and outcome studies behind progressive disclosure and usability gates; it does not enable those structures by default.

## v3.0 Key Additions

- **Anti-sycophancy protocols**: DA agents score rebuttals 1-5 before conceding. No concession below 4/5. Frame-lock detection.
- **Intent detection**: Socratic Mentor classifies user intent as exploratory vs. goal-oriented. Exploratory mode disables auto-convergence.
- **Cross-model verification** (optional): Set `ARS_CROSS_MODEL` env var to enable a non-Anthropic verifier (currently GPT-5.5 / GPT-5.5 Pro or Gemini 3.1 Pro) for integrity sample checks, a blind and separately executed Devil's Advocate critique, and blind disagreement checkpoints at design freeze + final editorial decision (#518). The once-planned generic sixth reviewer is retired, not deferred — see the "Why there is no generic 6th reviewer" note in `shared/cross_model_verification.md`, which also carries the supported-model table. These execution facts are not a binary independence claim.
- **AI Self-Reflection Report**: Pipeline Stage 6 now includes AI behavioral self-assessment (concession rate, health alerts, sycophancy risk rating).

## Histórico de versões anteriores

As notas de release de 25 versões **não ficam mais aqui**. Elas viviam neste arquivo em
seções `## vX.Y Key Additions` que repetiam o `CHANGELOG.md` — e este arquivo é carregado
por inteiro em **toda** sessão de todo agente. Histórico de release não muda a decisão de
quem está trabalhando agora; pertence ao lugar que se consulta sob demanda.

**Onde ler:** `CHANGELOG.md`. Antes de remover, cada uma das 25 foi conferida
individualmente contra a entrada correspondente — nenhuma se perdeu. A v3.7.3 está
documentada sob o cabeçalho `## [3.8.0] — L3 Claim-Faithfulness Locator + Audit
(v3.7.3 + #103 paired milestone)`, e não sob um `## [3.7.3]` próprio.

**Duas seções ficaram, e por motivo medido, não por descuido:**

- **a da versão corrente** (v3.20.1), porque o invariante 11 de
  `scripts/check_version_consistency.py` exige que a seção `## vX.Y… Key Additions` mais
  nova deste arquivo bata com a versão da suíte. Ao subir a versão, troque esta seção —
  não acrescente uma nova ao lado.
- **v3.0**, por duas razões independentes: `scripts/check_reviewer_role_label.py` fixa duas
  frases do bloco de verificação cruzada (`"a blind and separately executed Devil's
  Advocate critique"` e `"These execution facts are not a binary independence claim"`), e o
  `CHANGELOG.md` **não tem entrada alguma para a 3.0** — a entrada 3.x mais antiga é a
  `## [3.1.1]`. Aqui é o único registro que existe dessa versão. Vale abrir como pendência
  do repositório: enquanto a 3.0 não entrar no CHANGELOG, esta seção não pode sair.


## Routing Discipline (v3.9.2)

**Routing precedence:** This section runs BEFORE Routing Rules 1-5. Once this section settles on a destination, Rules 1-5 apply within that destination's skill family.

**Step 0 — Escape hatch check (before any classification):** If the user's first message begins with `[direct-mode]` (case-insensitive byte-0 token, optionally preceded by whitespace/newlines that are stripped on parse), record this fact, strip the prefix and surrounding whitespace from the message, and skip directly to **Step 1 explicit-intent handling** on the stripped content. The literal `[direct-mode]` is NOT passed through to the dispatched agent. If the stripped message itself has no clear skill named, Step 1 falls through to Step 3 clarification (the escape hatch bypasses cross-phase clarification (Step 2), not all routing).

Otherwise, classify the user's input:

1. **Explicit clear intent** — user invokes a specific skill via `/ars-*` slash command, or uses an unambiguous trigger keyword that maps to a single skill (e.g., "lit-review this", "review my paper", "draft an abstract"):
   → Route directly; no clarification, no orchestrator detour.

2. **Cross-phase materials detected** — user provides artifacts spanning ≥ 2 pipeline phases without naming a specific skill (e.g., pre-written abstract + pre-collected literature; full draft + reviewer comments + bibliography):
   → **Clarify**. Do NOT auto-route to a single-phase agent. List candidate workflows as a-d options in markdown body (NOT via AskUserQuestion tool). See `shared/references/intent_clarification_protocol.md` for the message template.
   → Reason: clarification is the safest action when materials don't unambiguously identify intent. (v3.10 active conductor (#134) will handle this via structured intake; v3.9.2 asks.)

3. **Ambiguous intent, no materials** — user provides no artifacts and no clear request:
   → Clarify per `shared/references/intent_clarification_protocol.md`.

**Anti-pattern (caused #133):** Receiving ambiguous cross-phase materials and silently auto-routing to a single-phase agent based on which phase the materials "look closest to." This bypasses orchestrator-level reconciliation and lets the subagent inherit the full ambiguity without independent oversight.

**Forward note (v3.10):** Active conductor (#134) will reframe this gate as structured intake with task envelope dispatch. v3.9.2 ships clarification-only as interim hot-fix.

## Routing Rules

1. **academic-pipeline vs individual skills**: academic-pipeline = full pipeline orchestrator (research → write → integrity → review → revise → final integrity → finalize). If the user only needs a single function (just research, just write, just review), trigger the corresponding skill directly without the pipeline.

2. **deep-research vs academic-paper**: Complementary. deep-research = upstream research engine (investigation + fact-checking), academic-paper = downstream publication engine (paper writing + bilingual abstracts). Recommended flow: deep-research → academic-paper.

3. **deep-research socratic vs full**: socratic = guided Socratic dialogue to help users clarify their research question. full = direct production of research report. When the user's research question is unclear, suggest socratic mode.

4. **academic-paper plan vs full**: plan = chapter-by-chapter guided planning via Socratic dialogue. full = direct paper production. When the user wants to think through their paper structure, suggest plan mode.

5. **academic-paper-reviewer guided vs full**: guided = Socratic review that engages the author in dialogue about issues. full = standard multi-perspective review report. When the user wants to learn from the review, suggest guided mode.

6. **rebuttal-audit vs revision-coach (input-shape gate)**: both touch reviewer comments, so route by INPUT SHAPE, not verbs. Route to `academic-paper rebuttal-audit` ONLY when the user supplies BOTH the reviewer comments AND an existing rebuttal/response draft to evaluate (it does advisory QA, generates nothing). If only reviewer comments are present (no draft yet), route to `revision-coach` (it generates a Response Letter Skeleton). If unclear which, clarify rather than guess. `rebuttal-audit` is standalone/advisory and never emits Schema 11 or marks anything verified.
7. **real-committee correspondence vs peer review (#668)**: route to the `revision-coach` committee-correspondence variant only when the user explicitly identifies a real committee or institutional review office. Formal tone and words such as “required” do not establish authority. That branch loads `academic-paper/references/committee_correspondence_protocol.md`, preserves the source, and emits its separate concern tracker; it never emits reviewer priority/severity, Schema 11, a determination, or a resolution claim.

## Key Rules

- All claims must have citations
- Evidence hierarchy respected (meta-analyses > RCTs > cohort > case reports > expert opinion)
- Contradictions disclosed with evidence quality comparison
- AI disclosure in all reports
- Default output language matches user input (Traditional Chinese or English)

## Full Academic Pipeline

```
deep-research (socratic/full)
  → academic-paper (plan/full)
    → integrity check (Stage 2.5)
      → academic-paper-reviewer (full/guided)
        → academic-paper (revision)
          → academic-paper-reviewer (re-review, max 2 loops)
            → final integrity check (Stage 4.5)
              → academic-paper (format-convert → final output)
                → Process Summary + AI Self-Reflection Report
```

## Handoff Protocol

### deep-research → academic-paper
Materials: RQ Brief, Methodology Blueprint, Annotated Bibliography, Synthesis Report, INSIGHT Collection

### academic-paper → academic-paper-reviewer
Materials: Complete paper text. field_analyst_agent auto-detects domain and configures reviewers.

### academic-paper-reviewer → academic-paper (revision)
Materials: Editorial Decision Letter, Revision Roadmap, Per-reviewer detailed comments

## Version Info
- **Suite version**: 3.20.1 (per CHANGELOG.md)
- **Last Updated**: 2026-08-15
- **Author**: Cheng-I Wu
- **License**: CC-BY-NC 4.0
