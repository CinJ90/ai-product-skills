# Contributing

Contributions that make product requirements clearer, more testable, or more useful across a delivery team are welcome.

## Proposing a change

1. Open an issue describing the product problem or observed skill behavior.
2. Keep the change focused on one skill or one evaluation concern.
3. Add or update a realistic example when the behavior changes materially.
4. Add an evaluation case that would have exposed the original gap.
5. Validate each changed `SKILL.md` before opening a pull request.

## Skill quality bar

A skill should:

- have a short, discriminating `name` and `description` in YAML frontmatter;
- preserve the user's intent and authorization boundaries;
- separate known facts from assumptions and recommendations;
- include only instructions that materially improve the agent's decisions;
- link supporting references from `SKILL.md`;
- avoid unfinished placeholders and duplicated guidance; and
- be supported by realistic, outcome-based evaluations.

Please avoid eval assertions that depend on exact headings or wording. Score the usefulness, completeness, and factual discipline of the result instead.
