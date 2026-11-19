# Task workflow with Codex

## 1. Define the task

A Trello card describes the goal and a few verifiable acceptance criteria. Before implementation, review the card, the existing project, and open questions. Clarify material uncertainties; Codex can make small, reversible decisions with a brief explanation.

Example task request:

> Work on Trello card <link>. Implement the next small, verifiable step and explain the C++ and Unreal concepts involved. Keep detailed documentation in the repository and link it in the result.

If Codex should also maintain Trello, explicitly include this in the request, for example: "Move the card to the appropriate working list at the start and add a result comment when finished."

Write documentation and pull request titles and descriptions in English. Write all authored Trello content in German, including card titles, descriptions, checklists, and comments. Communicate with the user in German.

## 2. Implement and learn incrementally

Codex explains the next step and the immediately relevant concepts, implements it, and explains important decisions using the actual code. Limit new systems to current prototype needs.

Keep setup instructions, technical guides, and lasting design decisions under `docs/`. Create documents when concrete content is available. The README remains the project entry point.

## 3. Verify the result

Choose verification appropriate to the change: documentation and link checks, a C++ build, or a concrete playtest in the Unreal Editor. Describe outstanding manual steps so the user can reproduce them. A test that has not run has not passed.

A task is complete when its agreed acceptance criteria are met, available checks have been performed, and relevant documentation is updated. If Editor checks or playtests remain outstanding, describe the result as ready for verification.

## 4. Record progress

The final response describes the result, verification, outstanding issues, and relevant file links. When Trello maintenance is requested, add only a short German summary with links to the detailed documentation. Keep status and acceptance criteria on the card and detailed technical content in the repository.

## Game language and future translations

Develop the game in English first. All player-facing text, including menus, settings, HUD elements, interaction prompts, dialogue, and mission text, starts in English. German translations are planned for a later stage and are not required for the initial prototype.

Keep player-facing text separate from gameplay logic and use localization-ready text mechanisms when implementing it, so German translations can be added later. Build only the localization support needed by the current feature; a complete translation workflow is not part of the initial setup.

## Current starting point

The repository includes `GTAHamburg.uproject`, the base Unreal Engine 5.8.4 Third Person C++ template, and its required assets. See [the setup guide](unreal-setup.md) for development tools, build/start commands, learning notes, and the current verification record. Check that record before assuming a build or playtest has passed.
