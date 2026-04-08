Here is the recommended four-phase research path to extract maximum value for our [] CLI PRD:


---



## Phase 1: Dissecting the "Heart" and "Limbs" (MVP Logic)

Before thinking about complex concurrency and ecosystems, understand the lowest-level mechanics of the agent.

* **`s01 THE LOOP` (`src/query.ts`)**
  - Why read it: This is the absolute core of the system. Look at the infinite `while-true` loop. Understand how the LLM decides whether to continue calling tools or return results to the
user based on the `stop_reason`.

* **`s02 TOOL DISPATCH` (`src/Tool.ts` & `src/tools.ts`)**
  - Why read it: Study how 40+ tools are abstracted into a unified interface (validation, authorization, execution, rendering). Understanding this factory pattern explains how the Agent  
grows "tentacles" to manipulate the file system and command line safely.

---

## Phase 2: Studying the "Moat" and Cost Control

LLM Context is extremely expensive. The biggest difference between a toy Agent and a production Agent is memory management.

* **`s06 CONTEXT COMPRESSION` (`src/services/compact/`)**
  - Focus: Deeply compare their three strategies:
    - `autoCompact`: Using API to summarize old messages
    - `snipCompact`: Hard-pruning useless markers
    - `contextCollapse`: Restructuring context
  - Understand how they prevent API bill explosions while maintaining an extremely fast Time-To-First-Token (TTFT).

---

## Phase 3: Exploring Advanced Forms & Autonomous Orchestration (Swarm)

This is the most ambitious and unorthodox part of the architecture, showing how a monolithic Agent evolves into multi-agent collaboration.

* **`s04, s09, s11`** (Sub-Agents & Autonomous Teams)
  - The Underlying Magic (`src/utils/swarm/backends/TmuxBackend.ts`): See how they cleverly hijack existing `tmux` or `iTerm2` sessions on the developer's machine to open hidden split    
panes. This achieves physical isolation and parallel execution for Agents without complex container orchestration.
  - Communication Protocol (`src/tools/SendMessageTool.ts`): Study how different Agents "negotiate" and dispatch tasks to one another using a standard request-response pattern.

---

## Phase 4: Elevating Strategic Intent (The [] Counter-Strategy)

Once you have a feel for the underlying code, step out of the codebase immediately. Return to the top-level `docs/` directory and examine these technical implementations from a macro     
perspective to formulate our counter-attack.

* **Remote Control & Killswitches** (`docs/en/04-remote-control-and-killswitches.md`)
  - Insight: See how enterprise tools implement hot-update risk control and remote degradation. This concerns the compliance lifeline of the product.

* **Future Roadmap** (`docs/en/05-future-roadmap.md`)
  - Insight: Focus on deconstructing *KAIROS*. Understand why they want to move from a "request-driven CLI" to an "event-driven background Daemon."
