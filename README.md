# Flowbit Plugin for GitHub Copilot CLI

This repository is packaged as a GitHub Copilot CLI plugin.

## What it provides

- SDLC workflow skills (development, performance, migration, research, product design)
- Specialized agents for analysis, planning, implementation, and verification
- Command-style entry skills copied from `commands/` into `skills/` for Copilot CLI loading

## Install from marketplace (recommended)

Add the Flowbit marketplace:

```bash
copilot plugin marketplace add plucins/flowbit
```

Browse available plugins:

```bash
copilot plugin marketplace browse flowbit
```

Install Flowbit from marketplace:

```bash
copilot plugin install flowbit@flowbit
```

## Install directly from repository

This Copilot CLI build installs plugins from a repository source (not from `.` path).

Install from GitHub repository:

```bash
# HTTPS
copilot plugin install https://github.com/plucins/flowbit

# SSH
copilot plugin install git@github.com:plucins/flowbit.git
```

Install from repository subdirectory:

```bash
copilot plugin install https://github.com/plucins/flowbit:plugins/flowbit
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

After pushing changes, update the installed plugin:

```bash
copilot plugin update flowbit
```
