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
- [`code-review`](./skills/engineering/code-review/SKILL.md) — review a diff against repository standards and its originating specification.

### Supporting skills

- [`grilling`](./skills/productivity/grilling/SKILL.md)
- [`domain-modeling`](./skills/engineering/domain-modeling/SKILL.md)
- [`codebase-design`](./skills/engineering/codebase-design/SKILL.md)
- [`research`](./skills/engineering/research/SKILL.md)
- [`prototype`](./skills/engineering/prototype/SKILL.md)
- [`setup-project-skills`](./skills/engineering/setup-project-skills/SKILL.md)

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

## Local customizations

### `code-review`

The upstream standards-and-spec review is extended with:

- Fowler's `Lazy Element` smell.
- An Evans/Vernon-inspired story group.
- `Untold Story` and `Scattered Story` findings.
- Standards-subagent instructions that apply the story group during review.

### `codebase-design`

The upstream deep-module vocabulary is extended with:

- `Arrangement`, `Story`, and `Discoverability`.
- Guidance for module naming and nesting.
- The story and concatenation tests in [`STORYTELLING.md`](./skills/engineering/codebase-design/STORYTELLING.md).

## Structure

```text
skills/
  engineering/
    code-review/
    codebase-design/
    domain-modeling/
    grill-with-docs/
    improve-codebase-architecture/
    prototype/
    research/
    setup-project-skills/
    tdd/
    to-spec/
    to-tickets/
    wayfinder/
  productivity/
    grill-me/
    grilling/
```

Each skill is installed as a complete directory so its relative references and supporting documents remain intact.

## Upstream and license

The initial curated subset is derived from [`mattpocock/skills`](https://github.com/mattpocock/skills) at commit [`2ab958093e83e0ec752e6c1c5932da465bf23e0c`](https://github.com/mattpocock/skills/commit/2ab958093e83e0ec752e6c1c5932da465bf23e0c).

The upstream and modified skill files are distributed under the MIT License. See [LICENSE](./LICENSE) and [NOTICE.md](./NOTICE.md).
