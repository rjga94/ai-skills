# AI Skills

This repository contains reusable skills for AI coding agents. Each skill is kept in its own top-level directory and documented in a `SKILL.md` file.

## Available skills

| Skill | Description |
| --- | --- |
| [software-engineering-standards](software-engineering-standards/SKILL.md) | Apply software engineering and architecture guidance when designing, implementing, refactoring, testing, or reviewing software, with platform-specific guidance for Kotlin Multiplatform and Apple projects. |

## Use a skill

1. Choose the skill directory you want to use.
2. Copy the entire directory into the skills location supported by your AI coding agent. Check that agent's documentation for the correct location and whether it needs to be reloaded.
3. Ask the agent to use the skill by name, or follow the activation steps provided by that agent.

Copy the whole directory rather than only `SKILL.md` so any supporting files included with a skill remain available.

## Add or update a skill

- Keep each skill in a top-level directory named for the skill, with its instructions in `SKILL.md`.
- Preserve the skill's metadata and supporting files when copying or updating it.
- Update the **Available skills** table whenever a skill is added, removed, renamed, or its purpose changes.

This repository does not currently define build, test, or lint commands.

## License

This project is licensed under the [MIT License](LICENSE).
