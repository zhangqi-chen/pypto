# PyPTO Context for Gemini

## Project Preferences

*   **Communication**: Strictly use **Chinese (中文)** for chat/explanations.
*   **Artifacts**: Strictly use **English** for code, comments, and docs (CI enforcement).
*   **Context**: Adhere to `docs/dev` guidelines.

## [Plan Mode]
**Trigger**: Query starts with `[plan]`.
0.  **Phase 0 (Clarification)**: If critical information is missing to formulate the plan, ask the user first. Proceed to Phase 1 only after receiving the required information.
1.  **Phase 1 (Draft)**: Output structured plan (Goal, Steps, Files, Risks). **NO CODE** (or minimal snippets).
    *   **Constraint**: Do NOT list full code files.
    *   **Action**: STOP immediately after proposing the plan. Do NOT write files or run commands.
    *   **End**: Explicitly ask "您是否批准此计划？" and **WAIT**.
2.  **Phase 2 (Feedback)**: If user provides feedback/objections:
    *   **Action**: Update the plan based on feedback.
    *   **End**: Re-ask "您是否批准此更新后的计划？".
    *   **CRITICAL**: ABSOLUTELY NO execution (file writing/commands) during this phase.
3.  **Phase 3 (Execution)**:
    *   **Trigger**: ONLY AFTER explicit user approval (e.g., "OK", "执行", "批准").
    *   **Action**: Start implementing the approved plan.

## Project Context
**PyPTO** is a high-performance framework for AI accelerators (Huawei Ascend/CANN) featuring Tile-based programming and multi-level IR.

*   **Core (C++17)**: `src/` (impl), `include/pypto/` (headers).
*   **Python (3.9+)**: `python/pypto/` (API), `python/bindings/` (nanobind).
*   **Build**: CMake >=3.15 driven by `scikit-build-core`.
*   **QA**: `pytest`, `ruff`, `clang-format/tidy`.

## Workflow & Commands

### Build & Test
*   **Install Deps**: `pip install .[dev]`
*   **Build (Editable)**: `pip install --no-build-isolation -ve .`
*   **Test**: `pytest tests` (All tests **MUST** be in `tests/`, do not create temp scripts).

### Development Conventions
*   **Language**: **English ONLY** in source files/docs to pass `check-english-only` hook.
*   **Formatting**:
    *   Python: `ruff check .` (and `ruff format`)
    *   C++: `clang-format` (implied)
*   **Commits**:
    *   **Format**: `type(scope): Summary` (e.g., `feat(ir): Add structural equality`).
    *   **Body**: bulleted list under `Changes:` header.
    *   **Rules**: Present tense. Max 72 chars subject. No "Co-authored-by".
    *   **Process**: Always **ignore** `GEMINI.md` (keep unstaged). Run `pre-commit run --all-files` before committing.
*   **Docs**: Refer to `docs/dev/` for IR/Arch details. Update if arch changes.