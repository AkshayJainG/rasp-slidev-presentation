# Task Plan: RASP Slidev Presentation

## Goal
Create and technically verify a polished Slidev presentation covering three Android RASP case studies: Promon Shield, an opaque commercial-style RASP referred to as RASP B, and Protectt.ai.

## Phases
- [x] Phase 1: Plan and setup
- [x] Phase 2: Research / gather information
- [x] Phase 3: Build the Slidev deck
- [x] Phase 4: Technical verification and three-case integration
- [x] Phase 5: Review and deliver

## Key Questions
1. What is the strongest narrative arc across the three engines?
2. Which technical details deserve slides, and which belong in speaker notes?
3. How can the deck look polished without feeling gimmicky?

## Decisions Made
- Standalone deck in `rasp-slidev-presentation` to avoid overwriting existing talk notes.
- Slidev Markdown because the material is technical, code-heavy, and versionable.
- Dark editorial / security-research aesthetic with diagrams, compact comparison tables, and restrained code excerpts.
- RASP B is intentionally anonymised: no project name, package, or library identifiers in the deck.
- Result language for RASP B describes a reliable analysis window rather than indefinite survival, to stay accurate about the direct-syscall termination behaviour observed.

## Status
**Complete** — three-RASP deck is technically verified and builds successfully.
