---
name: classic-asp-access-sql
description: Format and review SQL string construction for Classic ASP/VBScript projects that target Microsoft Access/Jet/ACE databases. Use when editing `.asp` or `.inc` files with SQL assembled via string concatenation, especially when joins, line continuations, aliases, `IIf`, `IsNull`, `DateAdd`, `DateValue`, or `TimeValue` are involved.
---

# Classic ASP Access SQL

Use this skill to write readable, MS Access-compatible SQL strings in Classic ASP.

## Core Rules

- Build long SQL with VBScript concatenation: one SQL fragment per line, ending continued lines with `& _`.
- Keep spaces inside string fragments so joined fragments do not merge tokens.
- Use table aliases consistently, but keep them short and readable.
- For MS Access joins, wrap each joined table/view pair in parentheses.
- When chaining joins, nest the parentheses around the accumulated join expression.
- Keep `WHERE`, `GROUP BY`, `HAVING`, and `ORDER BY` outside the final join parentheses.
- Prefer Access functions already used in the codebase: `IIf`, `IsNull`, `DateAdd`, `DateValue`, `TimeValue`, `Now`.
- Preserve existing helper conventions such as `sid`, `notnull`, `savesql`, `Db_OpenX`, `Db_Get_Value_X`, and `Db_XQ`.

## Join Parentheses

For two tables:

```asp
sql = _
   "SELECT A.id, B.naam " & _
   "FROM (tb_a A INNER JOIN tb_b B ON A.b_id = B.id) " & _
   "WHERE A.id=" & targetid
```

For three tables:

```asp
sql = _
   "SELECT A.id, B.naam, C.status " & _
   "FROM ((tb_a A " & _
   "INNER JOIN tb_b B ON A.b_id = B.id) " & _
   "LEFT JOIN tb_c C ON B.c_id = C.id) " & _
   "WHERE A.id=" & targetid
```

For four tables:

```asp
sql = _
   "SELECT A.id, B.naam, C.status, D.email " & _
   "FROM (((tb_a A " & _
   "INNER JOIN tb_b B ON A.b_id = B.id) " & _
   "LEFT JOIN tb_c C ON B.c_id = C.id) " & _
   "INNER JOIN tb_d D ON A.deelnemer_id = D.id) " & _
   "WHERE D.status_id=" & USER_STAT_ACTIVE
```

## Review Checklist

- Confirm every `JOIN` chain is parenthesized for Access, not only ANSI-style.
- Confirm closing parentheses appear before `WHERE`, not after it.
- Confirm each SQL fragment has leading or trailing spaces where needed.
- Confirm dynamic numeric values use existing numeric sanitizers such as `sid`/`sidr` where appropriate.
- Confirm dynamic strings/dates use existing helpers such as `savesql`.

## Avoid

```asp
sql = "SELECT ... FROM tb_a A INNER JOIN tb_b B ON ... INNER JOIN tb_c C ON ..."
```

Use the Access-safe parenthesized form instead:

```asp
sql = _
   "SELECT ... " & _
   "FROM ((tb_a A " & _
   "INNER JOIN tb_b B ON ...) " & _
   "INNER JOIN tb_c C ON ...) "
```
