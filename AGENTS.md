# PrepSchool — Agent Instructions

Before architecture or implementation work, read:
1. `docs/product/faang-interview-simulator-PROJECT.md`
2. `docs/product/PRODUCT-DECISIONS.md`

These are the product source of truth.

## Core rules
- Preserve existing work and inspect before modifying.
- Never silently replace an existing framework or architectural decision.
- Prefer shared abstractions over track-specific duplication.
- Keep business logic outside UI clients where possible.
- Add tests with behavioral changes.
- Keep interfaces schema-driven and typed.
- AI agents are development tools and must not receive commit authorship or co-authorship attribution. Commits are authored by the human contributor responsible for reviewing and submitting the change. Do not add AI-agent `Co-authored-by` trailers or agent authorship metadata.

## Four first-class tracks
- Software Engineering
- Cloud / Platform Engineering
- Project & Program Management
- Local AI / LLM Systems

## Interview integrity
Preserve server-controlled clock/phases, server-controlled hint ladder, solution locking, anti-sycophancy, anti-hallucination, separate interviewer and scorer/debrief responsibilities, and evidence-backed scoring.

The live interviewer must not have access to protected reference solutions.

## Reaction system
Reaction/interaction signals may affect coaching, but must NOT directly modify interview scores.
Distinguish observation from inference. Do not claim psychological or medical diagnosis.
Raw camera/audio stays local and ephemeral by default unless recording is explicitly enabled.

## Coaching
Metacognitive, philosophical, Socratic, pragmatic, and evidence-based encouragement are allowed.
Task-specific technical guidance must pass through the hint controller.

## Clients
Both Web UI and Terminal/TUI are first-class and use common domain/application logic.

## External harnesses
The repository is intended for Codex, Claude Code, Gemini CLI, and other agent harnesses.
Machine interfaces should include CLI, HTTPS API, WSS, and MCP.
Never store harness authentication credentials inside this repository.

## Agent handoffs
After substantial work, leave a concise handoff under `artifacts/agent-handoffs/<agent>/` covering work performed, files changed, tests executed, decisions, unresolved issues, and recommended next task.
