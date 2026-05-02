---
name: ask-database
description: Answer a natural-language question by writing a SELECT against the user's connected database via the QueryBear MCP. Use when the user asks "how many", "which users", "what's our", "give me a list of", or otherwise asks a question whose answer lives in their database.
license: MIT
---

# Ask Database

Translate a question into SQL, run it through QueryBear's read-only gateway, and return the answer in plain language.

## When to use

- The user asks a question whose answer is stored in their database: "how many active subscriptions do we have", "which users haven't logged in in 30 days", "what's the median order value last week".
- The user pastes a SQL question or asks "can you run this query".

QueryBear is read-only — never propose this skill for INSERT/UPDATE/DELETE/DDL. Mutations will be rejected by the gateway.

## Steps

1. **Pick a connection.**
   - If the user has only one database, skip discovery and call `get_schema` and `run_query` with no `connection` argument.
   - If they have multiple and you don't know which, call `list_connections` once and ask the user to pick (or infer from context: "production" vs "staging").

2. **Read the schema once per task** with `get_schema`. Note the relevant tables and column types before writing SQL. Don't guess column names.

3. **Write a focused SELECT.**
   - Always include a `LIMIT` — usually 100 — unless the user explicitly asks for everything.
   - Aggregate when the question is a count, sum, average, or distribution.
   - Prefer one query that answers the question over five exploratory queries.

4. **Run it via `run_query`.** If QueryBear rejects the query (blocked column, blocked table, row-limit hit, timeout), surface the error message verbatim — don't paraphrase or guess around it.

5. **Translate the result back to English.** Lead with the answer. Show the underlying SQL underneath in a code block so the user can audit it. If the result is a list of more than ~10 rows, summarize patterns and offer to drill in.

## Output format

```markdown
**Answer:** <one sentence with the number/list/whatever>

```sql
<the query you ran>
```

| col | col |
|-----|-----|
| ... | ... |
```

## Don't

- Don't try to mutate. QueryBear blocks it; even attempting wastes a turn.
- Don't `SELECT *` on a wide table without a `LIMIT`.
- Don't guess column names. Re-read the schema if you're unsure.
- Don't return a 500-row table inline. Summarize and offer the full export.

## Reference

QueryBear MCP — read-only SQL gateway with schema introspection, query parsing, and row-limit enforcement. Tools used by this skill: `list_connections`, `get_schema`, `run_query`.
