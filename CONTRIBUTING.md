# Contributing to Guard AI

## Branch Convention

### Permanent branches

| Branch | Role | Direct push |
|--------|------|-------------|
| `main` | Production — stable, deployed | No |
| `staging` | Integration — recette before prod | No |

### Working branches

All working branches must target `staging` via Pull Request.

```
feat/<scope>    ← new feature
fix/<scope>     ← bug fix
chore/<scope>   ← maintenance, deps, config
docs/<scope>    ← documentation
```

Examples:
```
feat/contracts/new-event-type
fix/contracts/motion-event-schema
chore/contracts/add-json-validation
docs/contracts/event-format-reference
```

### Flow

```
feat/* ──PR──► staging ──PR──► main
fix/*  ──PR──► staging ──PR──► main
```

## Pull Request Rules

- Target `staging`, never `main` directly
- PR title must follow: `type(scope): short description`
- Squash merge preferred to keep history clean
- At least 1 review required before merge to `staging`
- At least 1 review required before merge to `main`

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat(events): add face-recognized event schema
fix(door): fix missing required field in door-opened
chore(validation): add JSON schema validation scripts
docs(events): document all event types
```
