---
description: "Rewrite prose in plain language: strip AI writing tics from a file, a glob, or the last thing written"
argument-hint: "[--check] [FILE|GLOB ...]"
allowed-tools:
  - Read
  - Edit
  - Write
  - Glob
  - Grep
  - Bash
---

# /unslop:fix — plain-language rewrite

Read `${CLAUDE_PLUGIN_ROOT}/skills/unslop/SKILL.md` first and follow it exactly.
Keep `${CLAUDE_PLUGIN_ROOT}/skills/unslop/references/phrasebook.md` open for
substitutions.

## Target

`$ARGUMENTS` selects what to rewrite:

- **One or more paths or globs** — rewrite each file.
- **`--check` anywhere in the arguments** — report only. Change nothing on disk.
- **No arguments** — rewrite the file you most recently created or edited this
  session. If you haven't touched a file, rewrite your own last response and
  print the result. If neither applies, ask which file.

Only ever touch files the arguments name. Never rewrite a whole repo because a
glob was broad — if a glob matches more than 10 files, list them and ask first.

## Steps

1. **Detect.** Run the grep detector from the skill against each target to find
   candidate lines. Read every hit in context; some are legitimate usage.
2. **Rewrite.** Apply the skill's procedure to the prose. Leave code blocks,
   identifiers, log lines, quoted text, and required legal wording untouched.
3. **Verify.** Re-run the detector on the result. Every remaining hit needs a
   reason you can state.
4. **Report.** For each file, list the tics you removed by number and name. If
   you left a hit in place, say which and why.

## Never

- **Never invent a specific to replace a vague one.** If the plain version needs
  a number, filename, or consequence the source doesn't contain, keep the
  general wording and flag it as needing a fact. Say so in the report.
- Never drop a caveat, a number, or a failure condition to make a sentence read
  more smoothly.
- Never rewrite prose the user wrote themselves unless they named that file.

## Report format

```
FILE  — n tics removed
  1 antithesis          "isn't merely X — it represents Y"  →  "..."
  2 borrowed-jargon     "load-bearing assumption"           →  "..."
  kept: "idempotent" (term of art, defined on first use)
  needs a fact: migration time — source never says how long
```

With `--check`, print the same report with a `would fix` column and stop.
