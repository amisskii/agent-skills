# Podman Desktop agent skills

Agent Skills are reusable instructions and
resources that help coding agents work with the Podman ecosystem. Install skills
from this repository with the [skills CLI](https://github.com/vercel-labs/skills)
or [GitHub CLI](https://cli.github.com/manual/gh_skill_install).

## Available skills

| Skill                                                                  | Description                                                                                                                         |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [containerfile-image-names](skills/containerfile-image-names/SKILL.md) | Review Containerfile and Dockerfile base-image names for explicit registries, distinguishing build stages and unresolved variables. |

## Prerequisites

For the `npx skills` commands below, install:

- [Node.js](https://nodejs.org/) 22.20.0 or later, with npm and npx.
- [Git](https://git-scm.com/downloads).
- A [supported coding agent](https://github.com/vercel-labs/skills/blob/main/docs/agents.md),
  such as Claude Code or Codex.

Individual skills may have additional requirements described in their `SKILL.md`.

## Install skills

Run these commands from the project where you want to use the skills.

List the available skills without installing them:

```sh
npx skills add amisskii/agent-skills --list
```

Start an interactive installation, then select the skills and agents:

```sh
npx skills add amisskii/agent-skills
```

Install one skill, replacing `<name>` with a name from the table above:

```sh
npx skills add amisskii/agent-skills --skill "<name>"
```

To select every skill for Claude Code explicitly:

```sh
npx skills add amisskii/agent-skills --skill '*' -a claude-code
```

### Choose an agent

Use `-a` to select an agent. Repeat it to install for multiple agents:

```sh
npx skills add amisskii/agent-skills --skill "<name>" -a claude-code
npx skills add amisskii/agent-skills --skill "<name>" -a codex
npx skills add amisskii/agent-skills --skill "<name>" -a claude-code -a codex
```

Add `-y` to skip confirmation prompts when you have selected the skills and agents.

### Choose project or global scope

Installation defaults to the current project. Add `-g` to make a skill available
across projects for your user:

```sh
npx skills add amisskii/agent-skills --skill "<name>" -a claude-code -g
```

| Agent       | Project scope     | Global scope (`-g`) |
| ----------- | ----------------- | ------------------- |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex       | `.agents/skills/` | `~/.agents/skills/` |

The installer manages placement for each agent. Pass `--copy` to request copies
instead of symlinks where symlinking is supported.

## Use an installed skill

Ask your coding agent to use the skill with a relevant file. For example:

> Use containerfile-image-names to review the FROM instructions in my Containerfile.
> Report short image names and unresolved references without editing the file.

## Check for updates

Check installed skills for changes without installing updates:

```sh
npx skills check
```

Apply available updates:

```sh
npx skills update
```

Choose the scope when prompted, or specify it explicitly:

```sh
# Skills in the current project
npx skills check -p
npx skills update -p

# Skills installed globally
npx skills check -g
npx skills update -g
```

These commands operate on installed skills in the selected scope, including skills
from other repositories. To target one skill, use `npx skills update "<name>" -p`
or `npx skills update "<name>" -g`.

Unpinned installations follow the repository's default branch. Changes to a skill
merged into that branch can be installed with `update`; a release is not required.

## Pin a revision

To install skills from a specific revision, replace `<full-commit-sha>` with the
full 40-character Git commit SHA:

```sh
npx skills add "amisskii/agent-skills#<full-commit-sha>" --skill "<name>" -a claude-code
```

A commit pin keeps the installation on that revision when checking or applying
updates. Run `add` again with another SHA to change the pin, or without the `#…`
suffix to follow the default branch again. Keep the same agent and scope options
when replacing an installation.

## Install with GitHub CLI

[GitHub CLI 2.90 or later](https://github.com/cli/cli/releases/tag/v2.90.0) provides
an alternative installer. Select a skill interactively:

```sh
gh skill install amisskii/agent-skills
```

To select a skill, agent, and scope explicitly:

```sh
gh skill install amisskii/agent-skills "<name>" --agent claude-code --scope project
gh skill install amisskii/agent-skills "<name>" --agent codex --scope user
```

GitHub CLI uses `user` for global scope. Its default project scope requires a Git
repository. Use the same installer to manage subsequent updates:

```sh
gh skill update --dry-run
gh skill update
```

GitHub CLI has its own source tracking and revision-selection behavior. See its
[installation](https://cli.github.com/manual/gh_skill_install) and
[update](https://cli.github.com/manual/gh_skill_update) documentation for details.
