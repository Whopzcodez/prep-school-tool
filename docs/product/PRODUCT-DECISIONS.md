# PrepSchool — Product Decisions

This document extends `faang-interview-simulator-PROJECT.md`.

## Primary user
PrepSchool v1 is designed primarily for the individual candidate.

## First-class tracks
1. Software Engineering
2. Cloud / Platform Engineering
3. Project & Program Management
4. Local AI / LLM Systems

Primary emphasis is L3-L4, with selected Senior, Staff, Principal, and advanced project/program scenarios.

## Local AI / LLM Systems
Focus on consumer, workstation, and small-business hardware from approximately 12 GB to 512 GB aggregate accelerator/VRAM capacity.

Competencies include model selection, deployment, inference, serving, quantization, LoRA/QLoRA and other appropriate fine-tuning, RAG-vs-training decisions, evaluation, benchmarking, GPU/RAM capacity planning, multi-GPU execution, performance optimization, and troubleshooting.

## Practice modes
- LEARN
- REHEARSE
- ASSESS

The adaptive engine may recommend a mode, but the candidate can override it.

## Company model
Candidate Skill Model
→ Role + Level Competencies
→ Company Interview Configuration
→ Round Composition + Rubric Weights + Persona

Company configurations are overlays and do not fragment the candidate's underlying skill history.

## Initial vertical slices
- SWE-001 — live coding interview
- CLOUD-001 — production service degradation / troubleshooting incident
- PM-001 — at-risk launch with competing stakeholders
- LLM-001 — constrained local-LLM deployment, optimization, and evaluation scenario

## Deployment model
Hybrid architecture:
- browser-based web application
- first-class terminal/TUI application
- optional local lab agent for Docker, Git, shell, repositories, networking, GPU telemetry, and local AI runtimes

## Agent interoperability
Support Codex, Claude Code, Gemini CLI, and other compatible harnesses through:
- CLI
- HTTPS REST API
- WSS realtime/event streams
- MCP

Harness authentication stays outside the repository.

## HTTPS
HTTPS is the default non-local transport. WSS is used for realtime streams. Caddy is the initial reverse proxy.

## Reaction and coaching
PrepSchool may fuse:
- voice-derived interaction signals
- optional vision-derived interaction signals
- typing/tool activity
- reasoning/transcript signals
- interview events

Temporary interaction states may include:
STEADY, FOCUSED, HESITATING, SEARCHING, FRUSTRATION_SIGNAL, OVERLOAD_SIGNAL, RECOVERING, CONFIDENCE_SHIFT, BREAKTHROUGH.

These are interaction-state estimates, not psychological diagnoses.
Reaction states MUST NOT directly raise or lower interview scores.

## Observer privacy
Raw audio/video remains local and ephemeral by default. Persist raw recordings only when explicitly enabled.

## Coaching
Interviewer and coach are separate concepts. Coaching may provide evidence-based encouragement or metacognitive/philosophical/Socratic prompts. Any task-specific technical guidance must pass through the server-controlled hint controller.

## Reaction timeline
Session playback should combine interview events, code/tool state, transcript, signals, inferred reaction states, and coaching interventions while preserving the distinction between observation and inference.

## Evidence model
Observation → Evidence → Rubric → Score

## UX direction
Modern, minimal, high-information, low-clutter. The candidate workspace is primary; the AI interviewer does not dominate the screen.

Track-specific workspaces:
- SWE: problem / editor / tests
- Cloud: topology / terminal / logs / metrics
- PM: brief / roadmap / stakeholders / risks
- Local AI: terminal / models / GPU metrics / configuration / benchmarks / evaluation

## Shared engineering principle
PrepSchool should place candidates into constrained situations where their decisions produce measurable evidence about how they think.
