# Flowbit SDLC Plugin for GitHub Copilot CLI

This repository is packaged as a GitHub Copilot CLI plugin.

## What it provides

- SDLC workflow skills (development, performance, migration, research, product design)
- Specialized agents for analysis, planning, implementation, and verification
- Command-style entry skills copied from `commands/` into `skills/` for Copilot CLI loading

## Install

This Copilot CLI build installs plugins from a repository source (not from `.` path).

Install from GitHub repository:

```bash
copilot plugin install OWNER/REPO
```

Install from repository subdirectory:

```bash
copilot plugin install OWNER/REPO:plugins/flowbit-sdlc
```

Verify:

```bash
copilot plugin list
```

In an interactive Copilot CLI session, verify loaded components:

```text
/agent
/skills list
```

## Update after changes

After pushing changes, reinstall/update from the same source:

```bash
copilot plugin update flowbit-sdlc
```
