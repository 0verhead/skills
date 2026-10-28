# Engineering Skills

A curated set of composable agent skills for planning, specification, architecture, test-driven development, and code review.

This repository keeps the structure and installation conventions of [Matt Pocock's skills repository](https://github.com/mattpocock/skills), while including only the selected skills and their supporting skills. `code-review` and `codebase-design` contain local extensions, and the project setup skill has a neutral name.

## Included skills

### Primary skills

- [`grill-me`](./skills/productivity/grill-me/SKILL.md) — interview the user until a plan or decision is clear.
- [`grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — grill while maintaining domain language and decisions.
- [`wayfinder`](./skills/engineering/wayfinder/SKILL.md) — map work that is too large for one agent session.
- [`improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) — find and work through codebase deepening opportunities.
- [`tdd`](./skills/engineering/tdd/SKILL.md) — build in red-green-refactor slices.
- [`to-spec`](./skills/engineering/to-spec/SKILL.md) — turn the current conversation into a specification.
- [`to-tickets`](./skills/engineering/to-tickets/SKILL.md) — break a plan or specification into tracer-bullet tickets.
- [`implement`](./skills/engineering/implement/SKILL.md) — build one ticket or a small specification test-first, review it, and commit.
- [`implement-spec`](./skills/engineering/implement-spec/SKILL.md) — build a whole ticketed specification on an integration branch through parallel subagents.
- [`code-review`](./skills/engineering/code-review/SKILL.md) — review a diff against repository standards and its originating specification.

### Supporting skills

- [`grilling`](./skills/productivity/grilling/SKILL.md)
- [`domain-modeling`](./skills/engineering/domain-modeling/SKILL.md)
- [`codebase-design`](./skills/engineering/codebase-design/SKILL.md)
- [`research`](./skills/engineering/research/SKILL.md)
- [`prototype`](./skills/engineering/prototype/SKILL.md)
- [`setup-project-skills`](./skills/engineering/setup-project-skills/SKILL.md)

### Experimental skills

- [`find-code-reductions`](./skills/experimental/find-code-reductions/SKILL.md) — find code, interfaces, dependencies, ceremony, and tests the repository can stop owning.
- [`atomic-review`](./skills/experimental/atomic-review/SKILL.md) — verify large change sets through isolated reviewers and lead adjudication.

Upstream uses the spelling `domain-modeling`.

## Installation

Do not combine installation methods in the same environment, or the skills may be discovered more than once.

### Interactive subset installation

```bash
npx skills@latest add 0verhead/skills
```

Choose the skills and target agents interactively.

### Install every skill for Claude Code

```bash
npx skills@latest add 0verhead/skills \
  --skill '*' \
  --agent claude-code \
  --yes
```

### Install a complete Wayfinder subset

The standard skills installer does not resolve dependencies. Select supporting skills explicitly:

```bash
npx skills@latest add 0verhead/skills \
  --skill wayfinder grilling domain-modeling research prototype setup-project-skills \
  --agent claude-code \
  --yes
```

Project-local installation is the default. Add `--global` for a global installation. Add `--copy` if independent copies are preferred over the installer's default symlink mode.

To inspect the available names before installing:

```bash
npx skills@latest add 0verhead/skills --list
```

## Project setup

After installation, run this once in each project that will use the tracker-aware engineering skills:

```text
/setup-project-skills
```

It records the project's issue-tracker workflow, optional triage labels, and domain-document layout under `docs/agents/`.

## License

MIT. See [LICENSE](./LICENSE) and [NOTICE.md](./NOTICE.md).
