---
name: deploy-skills
description: Deploy or undeploy all local project artifacts (skills, commands, agents, hooks) to global ~/.claude/ via copy. SNAPSHOT versions are blocked from deployment.
version: 1.1.0
user_invocable: true
---

# Deploy Artifacts to Global Scope

Deploy artifacts from this project's root directories (`skills/`, `commands/`, `agents/`, `hooks/`) to the matching global `~/.claude/` directories via copy, making them available in any project.

SNAPSHOT versions (e.g. `1.2.0-SNAPSHOT`) are **blocked** from deployment. Remove the `-SNAPSHOT` suffix to deploy.

| Source directory | Global target         |
| ---------------- | --------------------- |
| `commands/`      | `~/.claude/commands/` |
| `skills/`        | `~/.claude/skills/`   |
| `agents/`        | `~/.claude/agents/`   |
| `hooks/`         | `~/.claude/hooks/`    |

## Arguments

- No arguments or `deploy` → deploy all stable artifacts
- `clean` → remove all artifacts from global that originated from this project

Parse `$ARGUMENTS`:

- If empty or `deploy` → run `./scripts/deploy.sh --all`
- If `clean` → run **Clean Flow** (below)

## Deploy Flow

Run the deploy script:

```bash
./scripts/deploy.sh --all
```

The script will:

1. Copy all stable (non-SNAPSHOT) artifacts to `~/.claude/`
2. Block any SNAPSHOT versions with a warning
3. Print a summary of deployed vs blocked artifacts

## Clean Flow

### Step 1: Remove deployed artifacts

For each of the four global directories (`~/.claude/commands/`, `~/.claude/skills/`, `~/.claude/agents/`, `~/.claude/hooks/`):

1. Find artifacts that match names in this project's source directories.
2. Remove the matching files/directories from `~/.claude/`.

### Step 2: Clean empty directories

After removing artifacts, remove any empty parent directories left behind. Do NOT delete `~/.claude/commands/`, `~/.claude/skills/`, `~/.claude/agents/`, or `~/.claude/hooks/` themselves even if empty.

### Step 3: Report summary

```
Clean Summary:

  commands/
    REMOVED:     ~/.claude/commands/child/generate-handwriting-practice.md
    REMOVED:     ~/.claude/commands/child/generate-handwriting-practice/
    CLEANED:     ~/.claude/commands/child/ (empty directory removed)

  skills/
    REMOVED:     ~/.claude/skills/create-business-docs/
    REMOVED:     ~/.claude/skills/extract-page-components/

  agents/
    REMOVED:     ~/.claude/agents/create-business-docs/
    REMOVED:     ~/.claude/agents/extract-page-components/

  hooks/
    (no artifacts found)
```

If no artifacts were found in any category, report: "No artifacts from this project were found in global ~/.claude/."
