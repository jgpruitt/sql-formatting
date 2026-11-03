# SQL formatting

An Agent Skill for writing and reviewing SQL in John Pruitt's preferred style.
It captures conventions developed over more than 20 years of writing SQL.

```sql
create table app.example
( id         uuid primary key
, name       text not null
, created_at timestamptz not null default now()
);

select
  r.id
, r.name
, r.created_at
from app.example r
where r.name is not null
and r.created_at >= _since
order by r.created_at desc, r.id;
```

## What it teaches

- Lowercase SQL syntax without changing literals or quoted identifiers.
- Leading underscores for new routine input parameters and local variables to
  avoid clashes with table/column names; existing names survive formatting.
- Leading commas and next-line declaration parentheses forming a left-hand
  punctuation gutter.
- Two-space nesting and clause-aligned queries.
- Leading boolean connectives, with parentheses preserving logical grouping.
- Selective compactness and alignment rather than rigid expansion or padding.
- SQL/PLpgSQL routine, CTE, DML, comment, and terminator conventions.
- Preservation of template markers, psql commands, and language boundaries.

The instructions explain why the conventions help: visible structure, local
edits, and readable diffs. They distinguish defaults from established local
variants. This is a formatting skill, not a database-design guide, query
optimizer, or executable formatter. It does not execute SQL or require access to any external project or
service.

## Install

### Skills CLI

```sh
npx skills add jgpruitt/sql-formatting
```

Choose your agent and installation scope when prompted. For a global pi install:

```sh
npx skills add jgpruitt/sql-formatting --skill sql-formatting --agent pi --global
```

The command is **`skills` (plural)**. The unrelated npm package named `skill`
does not implement `skill add <github-repo>`.

List the skill without installing it:

```sh
npx skills add jgpruitt/sql-formatting --list
```

If `sql-formatting` is already installed manually, back it up before replacing
it with an installer-managed copy.

### Manual pi install

Clone into pi's user-level skills directory:

```sh
git clone https://github.com/jgpruitt/sql-formatting.git \
  ~/.pi/agent/skills/sql-formatting
```

The destination must not already exist. In an active pi session, run `/reload`.
Invoke explicitly with:

```text
/skill:sql-formatting format path/to/query.sql
```

Pi can also select the skill automatically when authoring or editing SQL.

### Other agents

The repository follows the [Agent Skills specification](https://agentskills.io/specification).
Its root `SKILL.md` contains standard YAML frontmatter and Markdown instructions;
bundled references use relative paths. No pi-specific tools or runtime APIs are
required. Use the Skills CLI or your agent's documented skill installation method.

## Contents

| Path | Purpose |
| --- | --- |
| [`SKILL.md`](SKILL.md) | Formatting rules, rationale, and preservation workflow |
| [`references/examples.md`](references/examples.md) | Synthetic worked examples |
| [`evals/evals.json`](evals/evals.json) | Two reusable formatting smoke-test prompts |

The examples illustrate presentation; their types and routine attributes are
not universal schema recommendations.

## Validation

With [uv](https://docs.astral.sh/uv/) installed, run the Agent Skills reference
validator from the repository root:

```sh
uvx --from skills-ref agentskills validate "$PWD"
```

To check installer discovery locally:

```sh
npx skills add . --list
```

The evaluation prompts exercise table/CTE formatting and PL/pgSQL formatting,
including literal, delimiter, assignment-operator, and attribute preservation.
They are prompts for agent evaluation, not an automated SQL equivalence test.

## License

[MIT](LICENSE).
