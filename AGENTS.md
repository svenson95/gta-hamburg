# Working with Codex

## Project and language

- GTA Hamburg is a learning project using Unreal Engine 5 and C++. See `README.md` for its current scope and goals.
- Communicate with the user in German. Use conventional English names for code and identifiers.
- Write all documentation, including agent instructions, in English. Write pull request titles and descriptions in English.
- Write all Trello card titles, descriptions, checklists, comments, and other authored Trello content in German. Preserve code identifiers and links where needed.
- Develop all player-facing game text in English first, including menus, HUD, prompts, dialogue, and mission text. Plan for German translations later; keep text localization-ready as features are implemented. See `docs/workflow.md`.
- Work in small, understandable steps with concrete, verifiable results. Avoid architecture for features that are not needed yet.

## Tasks and documentation

- Trello tracks tasks, priorities, acceptance criteria, and progress. Read a linked card before starting work.
- Keep detailed technical and game design documentation in the repository: setup, decisions, controls, and learning notes. Add relevant documents under `docs/` when needed.
- Link cards and documentation instead of maintaining duplicate detailed content.
- Change Trello cards and publish comments only within the explicitly requested scope. If a status change was not requested, suggest the appropriate next status in the final response.
- Follow `docs/workflow.md` when working on a task.

## Learning C++ and Unreal

- Briefly explain the implementation goal and the relevant C++ and Unreal concepts. Connect explanations to the code actually changed.
- Distinguish standard C++ from Unreal-specific mechanisms, such as reflection macros, UObject lifecycle and garbage collection, Actors, Components, and Blueprints.
- Explain the reasons and main tradeoffs behind relevant decisions. Introduce concepts gradually and avoid unsolicited lengthy introductory courses.
- Deliver manageable, runnable steps. Include a concrete exercise or manual verification step when it helps understanding.
- Record lasting project-specific insights in repository documentation when useful.

## Implementation and verification

- Inspect the existing state before making changes and preserve the user's existing work.
- Respect the limited prototype scope and performance goals in the README. Justify new dependencies and complex systems using current requirements.
- Verify changes using appropriate available tools. Never claim builds or playtests passed unless they were run; explicitly report missing tools and outstanding checks.
- Finish by describing the result, verification, and any required Unreal Editor steps. Link the affected files.
