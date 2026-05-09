# Flowbit Plugin for GitHub Copilot CLI

This repository contains the Flowbit GitHub Copilot CLI plugin under `plugins/`.

## What it provides

- SDLC workflow skills (development, performance, migration, research, product design)
- Specialized agents for analysis, planning, implementation, and verification
- Plugin bundle located in `plugins/` (`plugin.json`, `commands/`, `skills/`, `agents/`)

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

Install from plugin subdirectory:

```bash
copilot plugin install https://github.com/plucins/flowbit:plugins
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
