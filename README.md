# multi-phase-memory-architecture-
A practical architecture for persistent, adaptive cognition in local AI assistants
# Multi-Phase Memory Architecture

**A practical architecture for persistent, adaptive cognition in local AI assistants.**

-----

## What this is

A working document describing an architectural pattern for local AI assistants where memory is processed in four parallel phases, inspired by biological sleep-wake cycles. It is written from the perspective of a practitioner running a production local assistant for daily personal and industrial use, not from the perspective of academic research.

The main artifact is this document — a practical guideline for implementation.

This repository is a companion to [persistent-cognition](https://github.com/fioruccione/persistent-cognition), which laid out the theoretical case for moving from volatile to persistent LLM cognition. Where persistent-cognition is the *why*, this repository is the *how*.

## Who it’s for

- Developers building local voice assistants (Ollama + Whisper stacks and similar) who have hit the ceiling of stateless turn-based interaction and are looking for next steps.
- Researchers interested in continual learning, memory consolidation, and sleep-inspired AI, who value grounded implementation over pure theory.
- Anyone building AI systems for a single user or narrow domain and questioning whether the mainstream preservation-of-general-capability paradigm is the right default.

It is **not** for those looking for ready-to-run code. This is architecture, not a library.

## Core proposals, in one paragraph

Local memory should not be a passive store queried reactively. It should be worked on by several concurrent loops operating during idle periods: a low-temperature reflection loop producing concise syntheses, a high-temperature oneiric loop producing creative associations, an adversarial filter promoting useful associations to longer-lived memories, and a nightly fine-tuning loop (LoRA) consolidating refined insights into the model weights themselves. 
A second proposal, orthogonal but equally important: personal assistants should be allowed to specialize over time, progressively losing generalist capabilities in favor of domain fit. **Catastrophic forgetting, treated in literature as a bug, is reframed here as adaptive resource allocation.**

## Real-World Insight: The Cognitive Rumination Problem

During live production testing on the Mac Mini branch, a critical architectural vulnerability emerged regarding the *Reflection Loop* (Loop 2a). If the agent enters an idle "sleep" state without a state-awareness check, it will continuously wake up, read the last conversation, synthesize it, and save it to the vector database. 

Over a single night of silence, this creates dozens of identical semantic duplicates in the Vector DB (Redis). We defined this as **Cognitive Rumination**. 

**The Fix (The "Dirty Flag"):** Sleep-cycle loops MUST NOT trigger purely based on time intervals. They require a strict *dirty flag* logic. The reflection loop must check a state pointer: *"Have new memories or conversations been logged since my last reflection?"* If false, the agent must immediately return to sleep, preserving computational resources and maintaining a dense, duplicate-free vector space.

## Status

- **First public draft:** April 2026
- **Implementation status:** Loop 1 (reactive) and Loop 2a (reflection with anti-rumination flag) in production on two hardware branches. Loops 2b, 2c scheduled for implementation in the coming months. Loop 3 (nightly LoRA) planned for late 2026.
- **Document status:** Live. Expected to evolve as implementation produces data. Revisions tracked via git log.

This repository is not a finished artifact. It is a record of thinking-while-building.

## Context of origin

The architecture described here was not designed top-down. It emerged from months of daily use of a local voice assistant named Euri, running on two independent hardware branches:

- Windows workstation with 2×RTX 4060 Ti, 96GB RAM, running Gemma 4 26B-A4B as primary production environment.
- Mac Mini M4 running Gemma 4 E4B as a resource-constrained experimentation environment.

The constraints of the Mac branch, in particular, forced architectural discipline that produced insights applicable to both environments. The principle *test on the weaker machine, optimize for the stronger one* proved repeatedly useful.

## A note on methodology

This document was co-designed through extended conversations with Claude (Anthropic) and Gemini (Google). Some of the clearer formulations — especially in the section on deliberate forgetting — emerged from dialog rather than from solitary writing. Credit is given where it matters.

That said, the responsibility for technical claims is mine. Errors are mine. The choice to publish is mine. 

## Contact and collaboration

Issues welcome for technical questions, critiques, and extensions. This is a working document — if you find a reference wrong, a reasoning unsound, or a known precedent I missed, open an issue.

If you are implementing something similar and want to compare notes: same channel.

## License

Content: CC-BY 4.0  
Code examples (when added): MIT

## Citation

If you find any of this useful in your own work:

```text
Fiorucci, S. (2026). Multi-Phase Memory Architecture for Local AI Assistants.
GitHub repository: [https://github.com/fioruccione/multi-phase-memory-architecture](https://github.com/fioruccione/multi-phase-memory-architecture)
