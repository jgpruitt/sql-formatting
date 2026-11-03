# Evidence and interpretation

## Source of the style

Derived from John's existing Memory Engine SQL style guide and all 83
tracked/discovered `.sql` files under the `memory-engine/` source checkout:
7,989 lines in total.
The reviewed snapshot was commit `3d0f3ad30987b04a98d7584ad058dc7a623b702c`
with a clean working tree. File enumeration used `rg --files` with `-g '*.sql'`,
confirmed against `git ls-files '*.sql'`; line counts used `wc -l`. Occurrence
observations below came from reading those files and whitespace/pattern scans,
not a committed parser-based style census.

The checkout was read only; no source SQL was reformatted as part of creating
this skill. Counts and line references describe that snapshot, not future versions.

Coverage:

| Area | Files | Lines |
| --- | ---: | ---: |
| `packages/database/core/` and `packages/database/auth/` | 47 | 4,319 |
| `packages/database/space/` and `packages/database/migrate/` | 14 | 2,765 |
| `queries/` | 22 | 905 |

Paths below are relative to `memory-engine/`. They are provenance references,
not runtime dependencies of this skill.

## Clear rules

- Lowercase syntax throughout the database files; preserve uppercase data such
  as `'DELETE'`, `'UTC'`, and error messages.
- Two-space structural nesting and leading commas across expanded list kinds.
- All 33 table declarations reviewed put the opening parenthesis on the next
  line, followed by the first column on that same parenthesis line.
- Expanded function signatures and return tables follow the table-list shape.
- CTE bodies normally use a standalone opening parenthesis, with comma-prefixed
  subsequent CTE declarations.
- Compact queries/calls coexist with expanded structures. There is no supported
  universal hard line-width limit.

The memory's table example uses the correct next-line opening parenthesis. Its
numbered prose incorrectly says "Opening paren on same line as table name".
The corpus and example take precedence over that sentence.

## Representative anchors

| Pattern | Source |
| --- | --- |
| Aligned table names/types, next-line opener, inline comments | `packages/database/auth/migrate/incremental/001_users.sql:5–13` |
| Unpadded table columns | `packages/database/core/migrate/incremental/001_space.sql:4–12` |
| Expanded signatures and return lists | `packages/database/space/migrate/idempotent/002_search.sql:10–43` |
| Expanded calls, lowercase procedural syntax, preserved literals | `packages/database/space/migrate/idempotent/002_search.sql:65–88` |
| Compact query, wrapped projection, long join, function wrapper | `packages/database/core/migrate/idempotent/004_space.sql:25–37, 64–89` |
| Clause-aligned `where` / `and` and expanded projection | `packages/database/core/migrate/idempotent/004_space.sql:143–163` |
| Expanded insert and values lists | `packages/database/space/migrate/idempotent/004_memory_event.sql:37–65` |
| Expanded update assignments | `packages/database/space/migrate/idempotent/001_memory.sql:704–712` |
| CTE comma gutter | `packages/database/core/migrate/incremental/012_default_groups.sql:22–47` |
| Expanded nested aggregate arguments | `packages/database/core/migrate/idempotent/008_api_key.sql:263–286` |
| Expanded case expressions | `packages/database/space/migrate/idempotent/001_memory.sql:529–550` |
| Standalone query projection and final-clause semicolon | `queries/01-fulltext.sql:15–29` |
| psql variables and query-buffer execution | `queries/_setup.sql:1–78` |

## Deliberate variants and isolated exceptions

### Boolean continuation indentation

Database routines predominantly put `and` at the same indentation as `where`.
The core/auth review found 79 adjacent `where` → `and` pairs aligned and only
four with the connective indented two extra spaces. Standalone query scripts,
however, consistently use the indented variant. The skill chooses flush clause
alignment for new SQL and permits an established local indented style.

### Alignment

Auth table definitions often align types and constraints. Early core tables and
many function signatures use single spaces. Alignment is therefore a selective
readability device, not a required whole-file grid.

### Compact and expanded forms

Compact SELECT lists, one-line mutations, and compact CTE queries are common.
Some SELECT lists start on the `select` line and put comma continuations beneath
the projection. Expanded `select` with a punctuation gutter is the clearest new
multiline default; the other form is not a violation.

### Procedural assignments and bodies

`=` dominates the space routines; `:=` is common in core and the function
signature helper. Preserve the local choice. Most `declare` / `begin` / `end;`
keywords align at column zero, but one API-key routine shifts the whole body
inward. Most closing delimiters share a line with attributes, while the queue
routine file puts `language ...` on the following line. Neither difference
justifies unrelated reformatting.

### Semicolons

Standalone semicolons are characteristic for function wrappers and long
procedural queries. Script queries often end on the final clause, and simple
DDL ends `);`. Single SQL-language body queries often omit an inner semicolon.
These variants should not be flattened into one inflexible rule.

### Exceptions not promoted into defaults

- A trailing-comma argument list in
  `packages/database/space/migrate/idempotent/001_memory.sql:595–605`.
- Eight tab-containing lines in a temporal check in
  `packages/database/space/migrate/incremental/001_memory.sql:40–47`.
- A right-aligned UPDATE keyword layout in
  `packages/database/auth/migrate/incremental/008_betterauth_1_7.sql:43–47`.
- Same-line CTE/subquery openers in diagnostic scripts and the signature helper.
- Imperfectly maintained column/alias alignment and occasional tight comma
  spacing. Infer the recurring convention, not accidental whitespace.

## What this skill intentionally does not infer

The original memory also recommends native UUID/UUIDv7 storage. That is schema
policy, not formatting. Likewise, schema templates, authentication models,
search operators, migration machinery, security attributes, search paths,
qualification requirements, extension types, and naming of parameters are not
universal formatting requirements. Preserve the SQL's actual choices and obey
the target repository's design instructions separately.
