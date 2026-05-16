---
type: Note
---
# Portent Vault Template

A starter vault template with the default Portent type definitions.

Portent is an opinionated knowledge model built around eight object types, default relationships, and a simple lifecycle:

- capture information to make it available
- organize it with types and relationships to make it actionable
- archive it when it should no longer appear in active views

## Type Documents

The default Portent type definitions are:

- [[project]]
- [[operation]]
- [[responsibility]]
- [[task]]
- [[event]]
- [[note]]
- [[topic]]
- [[person]]

These files are written as Markdown type documents so local-first tools such as Tolaria can use them directly.

## Default Relationships

Use `belongs_to` for primary context and `related_to` for secondary context.

```yaml
---
type: Event
state: organized
belongs_to: "[[Launch Portent v0.1]]"
related_to:
  - "[[Portent]]"
  - "[[Knowledge bases]]"
---
```
