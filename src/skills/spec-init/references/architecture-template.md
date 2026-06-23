# Architecture Document Template

Optional documentation — only generate if user selected "Architecture" in Phase 3.

```markdown
# System Architecture

## Overview
[High-level description of system architecture]

## Visual Architecture Context
[Mermaid diagram generated via `flowbit-diagrams-mermaid` that refines existing architecture content. Prefer C4Context/C4Container. Do NOT replace narrative sections.]

## Architecture Pattern
**Pattern**: [From analysis - e.g., "Layered monolithic with REST API"]

[Description of how the pattern is implemented]

## System Structure

### [Component 1]
- **Location**: [From analysis - e.g., "src/api/"]
- **Purpose**: [What it does]
- **Key Files**: [List from analysis]

### [Component 2]
- **Location**: [From analysis]
- **Purpose**: [What it does]
- **Key Files**: [List from analysis]

## Data Flow
[Describe how data flows through the system]

## Component Communication Flow
[Mermaid sequence/flow diagram generated via `flowbit-diagrams-mermaid` to clarify interaction order. Use only confirmed domain context.]

## External Integrations
[List integrations found in analysis - databases, APIs, services]

## Database Schema
[If ORM detected, reference schema file location]

## Configuration
[How configuration is managed]

## Deployment Architecture
[If detected - Docker, K8s, cloud services]

---
*Based on codebase analysis performed [Date]*
```
