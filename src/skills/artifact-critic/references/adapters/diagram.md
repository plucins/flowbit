# Adapter: diagram

## Focus questions

- Are system boundaries explicit?
- Are critical flows complete (including failure paths)?
- Are assumptions about external systems visible?
- Is the level of detail appropriate for the decision gate?

## Common failure patterns

- Happy-path only; no error or timeout branch.
- Missing ownership of components or responsibilities.
- Hidden coupling between subsystems.
- Diagram abstraction too high to validate implementation impact.

## Extra acceptance conditions

- Includes at least one failure/exception path.
- Defines boundaries and key interfaces clearly.
- Supports the stated decision with enough detail.
