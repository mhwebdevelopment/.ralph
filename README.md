# `.ralph` Agentic Development Setup

This directory defines the operational configuration, role-based behavioral constraints, and extensible capability matrix for an autonomous/semi-autonomous AI agent workflow. The setup is orchestrated via a Universal Agent System (UAS) architecture, enforcing strict semantic boundaries and execution rules across discrete operational modes.

## Architecture Overview

The system is structured into three core domains: `modes` (role definitions), `rules` (operational constraints), and `skills` (executable capabilities). The core engine is driven by the `UAS_*` schema, which dictates inter-agent communication, escalation pathways, and task delegation.

### 1. Modes (`.ralph/modes/`)
Defines specialized operational contexts. Each mode encapsulates specific responsibilities, permitted actions, and cognitive directives.

*   `orchestrator.xml`: The primary controller. Handles task breakdown, delegates assignments to specialized modes, and monitors overall execution lifecycle.
*   `product-manager.xml`: Focuses on requirement gathering, scope definition, user story generation, and acceptance criteria formulation.
*   `semantic-architect.xml`: Responsible for high-level system design, data modeling, API contract definition, and ensuring architectural consistency.
*   `implementer-backend.xml`: Specialized role for server-side logic, database interactions, API implementation, and infrastructure-as-code tasks.
*   `implementer-frontend.xml`: Specialized role for client-side execution, UI/UX implementation, state management, and asset integration.
*   `reviewer.xml`: Quality assurance role. Executes code reviews, enforces style guides, checks diffs, and validates against acceptance criteria.
*   `UAS_MODE.xml`: The foundational schema defining the abstract base class for all operational modes.

### 2. Rules (`.ralph/rules/`)
Global and schema-specific constraints enforcing deterministic behavior and safety guardrails.

*   `00-global.xml`: Base ruleset applicable to all agents regardless of active mode (e.g., token limits, formatting strictures, mandatory logging protocols).
*   `UAS_SCHEMA_ENGINE.xml`: The core validation engine enforcing the semantic structure of agent outputs and inter-mode handoffs.
*   `uas_schema_compiler.xml`: Directives for parsing, validating, and compiling dynamic schemas during runtime execution.

### 3. Skills (`.ralph/skills/`)
Modular, callable capabilities mapped to specific modes. Skills define *how* an agent interacts with the external environment or performs complex logical operations.

*   `orchestrate-task/`: Logic for parsing epics into discrete, assignable tickets.
*   `product-management/`: Tools for requirements parsing and PRD (Product Requirements Document) generation.
*   `implement-backend/` & `implement-frontend/`: Execution environments for code generation, patching, and syntax validation.
*   `review-diff/`: Tools for analyzing version control diffs and generating inline feedback.
*   `semantic-escalation/`: The critical protocol for resolving ambiguity. Allows a lower-level agent (e.g., implementer) to pause execution and query a higher-level agent (e.g., architect) when encountering undefined states.
*   `uas-engine/`: The core system dependencies (`UAS_MODE.xml`, `UAS_SCHEMA_ENGINE.xml`, `UAS_SKILL.xml`) mirrored for skill-level access and validation.
*   `UAS_SKILL.xml`: The base schema defining the interface for all executable skills.

## Core Interaction Loop (UAS)

1.  **Ingestion:** User input is processed by the `product-manager` or `orchestrator` depending on the input abstraction level.
2.  **Delegation:** The `orchestrator` utilizes the `UAS_SCHEMA_ENGINE` to format structured payloads and invoke specific `modes` (e.g., `implementer-backend`).
3.  **Execution:** The activated mode utilizes its permitted `skills` to generate artifacts or execute commands.
4.  **Escalation:** If confidence drops below a threshold, the agent invokes `semantic-escalation` to return control to the `orchestrator` or `semantic-architect`.
5.  **Validation:** Artifacts are routed to the `reviewer` mode via `review-diff` before final commit or output.

## Extensibility

To add a new capability:
1. Define the capability logic in a new directory under `.ralph/skills/`.
2. Ensure the skill conforms to the `UAS_SKILL.xml` interface.
3. Map the skill to the appropriate operational profile in the corresponding `.ralph/modes/*.xml` file.