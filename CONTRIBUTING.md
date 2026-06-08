# Contributing

Thanks for your interest in improving Architect Builder.

## Development Rules

- Keep skills small, explicit, and workflow-oriented.
- Do not add runtime dependencies unless they are necessary.
- Keep Codex local skill usage and Claude Code plugin usage both working.
- Update both `README.md` and `README.zh-CN.md` when changing user-facing behavior.
- Run Claude Code plugin validation before submitting changes:

```powershell
claude plugin validate .
```

## Skill Changes

When editing a skill:

1. Update the relevant `skills/<name>/SKILL.md`.
2. Keep trigger wording clear and narrow.
3. Avoid broad behavior that can override unrelated user intent.
4. Add examples only when they reduce ambiguity.

## Release Checklist

Before publishing a release:

- [ ] `claude plugin validate .` passes.
- [ ] README installation commands are current.
- [ ] `CHANGELOG.md` is updated.
- [ ] Version in `.claude-plugin/plugin.json` is updated if needed.
- [ ] Version in `.claude-plugin/marketplace.json` metadata is updated if needed.

