# GTA Hamburg

> An experimental third-person open-world game set in Hamburg, Germany, in 1938.

**GTA Hamburg** is a personal game development experiment built around the idea of exploring a historically inspired city through a fictional story. Inspired by the freedom and gameplay of classic *Grand Theft Auto* games, the project aims to combine open-world exploration, vehicles, character interactions, and missions with an interest in real history.

This is **a learning project and prototype**, not a promise to deliver a complete game. The goal is to explore game development with **Unreal Engine 5 and C++**, build small playable features, and see how far the project can progress with the help of AI-assisted development.

> **Note:** GTA Hamburg is an unofficial working title. This project is not affiliated with or endorsed by Rockstar Games or Take-Two Interactive.

## Vision

- **Setting:** Hamburg in 1938, shortly before World War II.
- **Perspective:** Third-person gameplay.
- **World:** A historically inspired, explorable urban environment.
- **Story:** An original fictional narrative grounded in researched historical context.
- **Gameplay:** Exploration, driving, NPC interactions, and story-driven missions.
- **Visual style:** Stylized 3D with a subtle cartoon look, while keeping environments, proportions, and movement believable.
- **Performance:** Smooth gameplay on modest hardware is a core design goal, not an afterthought.

The game should feel like a game rather than strive for photorealism. Historical accuracy and visual realism are separate goals: a stylized presentation can still portray places and events with care.

## Technology

| Area | Planned technology |
| --- | --- |
| Game engine | Unreal Engine 5 |
| Programming language | C++ |
| Gameplay logic | Primarily C++, with Unreal Blueprints where useful |
| Target platform | Windows PC (initially) |
| 3D assets | Blender and appropriately licensed assets (to be decided) |
| Project management | Trello |
| Version control | Git / GitHub |

The technical choices may evolve as the prototype develops.

## First milestone: Playable prototype

The initial scope is intentionally small: **one limited city district**, not an entire open world.

- [ ] Create and configure the Unreal Engine C++ project
- [ ] Implement basic third-person character movement and camera controls
- [ ] Build a small street environment
- [ ] Add one controllable vehicle, including entering and exiting
- [ ] Add a few simple NPCs and interactions
- [ ] Implement one short mission
- [ ] Measure performance and improve the most expensive systems

Future features will be evaluated based on what is learned from this first milestone. None of the items above should be considered a committed release schedule.

## Performance goals

The project should remain playable on older gaming PCs. Current development hardware provides an initial reference point:

- **CPU:** AMD Ryzen 5 5600X
- **GPU:** NVIDIA GeForce GTX 1660 Ti
- **RAM:** Not yet confirmed (estimated 8–16 GB)

An early **1080p / 60 FPS** performance target is aspirational and must be validated with representative scenes and packaged builds. Priorities include scalable graphics settings, efficient geometry and materials, level streaming, sensible NPC density, and avoiding unnecessary high-end rendering features.

## Historical setting

The setting is Germany under National Socialist rule in 1938. Historical events and social realities will be approached through research and contextualization. The fictional story and characters are not intended to replace or distort documented history.

The exact protagonist, narrative, locations, and historical events have not yet been decided.

## Development approach

Work will be organized into small, testable tasks in Trello and implemented incrementally, with AI-assisted planning, research, and coding. The repository will gradually collect technical notes, design decisions, and other documentation as they become useful.

**Current status:** Concept / pre-production. No playable build is available yet.

## Assets and licensing

The project will use original or appropriately licensed code, artwork, music, and sound effects. Assets and audio from existing GTA games are not part of this repository.

## Disclaimer

This is a non-commercial personal prototype at this stage. The name, scope, and direction may change during development.
