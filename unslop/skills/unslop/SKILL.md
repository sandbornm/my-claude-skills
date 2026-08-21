---
name: unslop
description: >-
  Use when writing or editing prose a person will read — docs, READMEs, PR
  descriptions, commit messages, reports, summaries, plans, code comments, chat
  replies — or when asked to "unslop", simplify, de-jargon, plain-language, or
  clean up text that sounds like a machine wrote it. Triggers: "it's not X, it's
  Y", "not just X but Y", "load-bearing", "byte-identical", "surface area",
  "leverage", "utilize", "delve", "it's important to note", "at the end of the
  day", stacked nouns, SAT words used for polish, hedge stacking, manufactured
  profundity, inflated significance, rule-of-three lists.
---

# Unslop

If you can't say it plainly, you don't understand it yet. Plain words are the
test of understanding, not a concession to a dumb reader.

Feynman's rule: explain it to a smart person who doesn't work in your field,
using ordinary words. What survives that translation is what you actually know.
What needs jargon to stand up was probably decoration.

Nobody has ever been impressed by a word they had to decode. They were slowed
down by it.

## What plain writing is

Four parts. Build each sentence out of them.

1. **A subject that does something.** A real actor, an active verb.
   `the wrapper catches errors`, not `errors are handled by the wrapper`.
2. **Concrete nouns.** Files, functions, numbers, people, error strings. Not
   category words: *solution, approach, framework, capability, experience,
   strategy, methodology, paradigm*.
3. **Ordinary words.** The word you'd use out loud to a colleague. Reach for a
   technical term only when it carries information the plain word loses.
4. **One idea per sentence.** Three `and`s means three sentences.

## The procedure

1. **Say what the sentence claims** in words you'd use in conversation. Write
   that down.
2. **Compare.** If the plain version loses something a reader needs — a number,
   a condition, a precise term — put that back. Otherwise the plain version wins.
3. **Replace, never just delete.** Cutting a flourish and leaving a hole makes
   the text vaguer, which is a worse outcome than the slop was.
4. **Read it aloud.** Anything you stumble over, rewrite.

## Keep these — always

Removing slop must not remove information. Never strip:

- Numbers, versions, filenames, function names, error strings, commands
- Real caveats and failure conditions ("this breaks on Windows paths")
- A term of art whose plain replacement would mislead the reader
- Direct quotes, and the user's own words in text they wrote
- Code, identifiers, and anything inside a code block

Plain is not vague. Plain means the reader knows exactly what you mean.

**Never invent a specific to replace a vague one.** Turning `significantly
faster` into `40% faster` is right only if you have the number. Turning `a
load-bearing assumption` into `the scheduler stops retrying` is right only if
you checked. When the plain version needs a fact you don't have, keep the
general wording or ask for the fact. A confident wrong number does more damage
than any amount of slop.

## The eleven tics

### 1. The antithesis — "it's not X, it's Y"

Forms: *It's not a bug, it's a feature. This isn't about speed — it's about
trust. Not just faster, but fundamentally different. Less a library, more a
philosophy.*

The construction invents a contrast so an ordinary claim feels like a
revelation. Half the sentence describes something you aren't claiming, and the
reader carries a negation to reach the point.

**Cut the "not" clause. State Y.** Keep the negation only when the text earlier
asserted X and you are correcting it.

> Before: The retry logic isn't a performance optimization — it's a correctness
> guarantee.
> After: Retries exist for correctness. They make nothing faster.

### 2. Borrowed-jargon modifiers — "the load-bearing X"

A metaphor imported from another field, glued to a plain noun to make it sound
engineered: *load-bearing, byte-identical, first-class, orthogonal, surface
area, the long pole, non-trivial, table stakes, step function, impedance
mismatch, the substrate, the primitive.*

**Say the consequence instead of the metaphor.**

| Slop | Plain |
|---|---|
| a load-bearing assumption | if this is wrong, the scheduler stops retrying |
| reduce the surface area | there are four public methods instead of twenty |
| non-trivial to migrate | about two days, because the IDs change format |
| the concerns are orthogonal | you can change one without touching the other |
| first-class support for X | X works directly, no plugin needed |
| a byte-identical rebuild | rebuilding gives the same file, byte for byte |

Test for a technical term: **replace it with its definition. If nothing is
lost, it was decoration. If a reader could now act wrongly, it was precise —
keep it and define it once, on first use.** `byte-identical` earns its place in
a reproducible-builds doc and nowhere else.

### 3. Stacked nouns

*A config-driven pipeline orchestration layer. A schema-validation-first
ingestion strategy.*

Stacking nouns deletes the verbs, and the verbs are where the action is. The
reader has to reverse-engineer who does what to whom.

**Unroll it into subject–verb–object.**

> Before: We adopted a config-driven approach to pipeline orchestration.
> After: A YAML file lists the steps. The runner reads it and runs them in order.

### 4. SAT words

*Utilize, leverage, facilitate, delve, myriad, plethora, commence, endeavor,
ascertain, requisite, cognizant, disparate, salient, elucidate, obviate.*

Every one has a shorter twin that means the same thing. Use the twin. The full
list is in [references/phrasebook.md](references/phrasebook.md).

Special case — **copula avoidance**: `serves as`, `functions as`, `acts as`,
`represents`, `constitutes` almost always mean **is**. Write `is`.

### 5. Throat-clearing

*It's important to note that. It's worth mentioning. Notably. Crucially.
Fundamentally. Essentially. At its core. At the end of the day. In today's
fast-paced world. Let's dive in.*

**Delete the phrase and keep the sentence.** Important things earn attention by
going first, not by being announced.

### 6. Empty amplifiers

*Significantly, substantially, dramatically, vastly, incredibly, truly,
genuinely, remarkably, quite simply.*

**Replace with the measurement.** `significantly faster` → `1.2s → 0.7s`. No
number available? Delete the word and leave the plain claim.

### 7. Inflated significance

*A fundamental shift in how we think about X. A paradigm shift. Redefines what
it means to Y. Stands as a testament to. Marks a turning point. Changes
everything about.*

The sentence claims importance instead of showing it. Readers decide what
matters; the writer's job is to say what changed.

**State the change and let the reader rank it.**

> Before: This middleware represents a fundamental shift in how we think about
> reliability.
> After: Every service now retries with the same backoff policy. Before, each
> one had its own.

### 8. Manufactured profundity

Short sentences engineered to land: *That's the whole trick. And that's the
point. Simple as that. That's it. Full stop. Which is exactly the problem.*
Same tic in paragraph form — the one-line paragraph used as a drumbeat.

**Delete it, or fold its content into the sentence before.** A conclusion that
holds up doesn't need a drumroll.

### 9. The rule of three

*Fast, reliable, and secure. Clean, simple, and maintainable.*

Triads sound complete, so the third item gets invented to fill the slot.

**Keep only the items you can back with evidence.** One item you can defend
beats three you can't.

### 10. Stacked hedges

*May potentially. Could possibly. Somewhat unclear. Generally tends to. It
seems likely that perhaps.*

**One hedge maximum.** Better: name the actual gap. `I haven't tested this on
Windows` beats `this may potentially have some issues on other platforms`.

### 11. Mirroring the question

*Great question! You're asking about X. Let me explain X.*

**Answer.** The reader knows what they asked.

## Worked example

**Before** — nine tics in three paragraphs:

> It's important to note that this middleware isn't merely a performance
> optimization — it represents a fundamental shift in how we think about
> reliability. By leveraging a config-driven approach to backoff policy
> orchestration, we obviate the need for disparate retry implementations
> scattered across numerous services.
>
> The design makes a load-bearing assumption about idempotency, which
> significantly reduces the surface area for correctness bugs. Migration is
> non-trivial, but the resulting system is fast, reliable, and maintainable.
>
> At the end of the day, retries may potentially fail. That's the whole point.

**After:**

> One backoff policy for every service. Before this, each service had its own
> retry code.
>
> The policy lives in a config file. The middleware reads it and applies the
> delays in order.
>
> Retries assume handlers are idempotent: running one twice does the same thing
> as running it once. A handler that isn't idempotent repeats its side effects
> on every retry.
>
> Migration is the slow part. Retry call sites move into the middleware one
> service at a time.
>
> Retries can still exhaust their attempts. When they do, the error reaches the
> caller.

Two decisions worth copying:

- **`idempotent` stayed.** Its plain replacement would mislead a reader deciding
  whether their handler is safe to retry, so it stays — defined once, in the
  sentence that introduces it.
- **Nothing got invented.** The original never said how many services or how
  long migration takes, so the rewrite doesn't either. It says migration is slow
  and what makes it slow, which is all the source supported.

## Applying this to your own output

Before sending a response or saving a document, reread it once against the
eleven tics. The two that survive self-review most often are the antithesis
(tic 1) and the borrowed-jargon modifier (tic 2) — they feel like insight while
you're writing them.

Grep is faster than memory for a long document:

```bash
grep -nEi "it'?s not .*,? it'?s|not just .*(but|—)|load-bearing|surface area|non-trivial|orthogonal|leverage|utilize|delve|myriad|plethora|serves as|functions as|it'?s (important|worth) (to note|noting|mentioning)|at the end of the day|significantly|substantially|that'?s the (whole )?(trick|point)|full stop|fundamental(ly)? shift|paradigm shift|stands as a testament|redefines what" FILE
```

Read every hit yourself. Some are real usage — `leverage` in a finance doc,
`orthogonal` in a linear algebra proof. The grep finds candidates, you decide.

## Red flags — you are writing slop right now

- You typed a word you would not say out loud to a colleague
- You reached for a second adjective because one felt thin
- You wrote a contrast to make a plain claim sound bigger
- You added a third list item to round out the rhythm
- You put a metaphor where a consequence belongs
- You wrote a short punchy sentence to make a paragraph land
- You wrote a number, filename, or consequence you did not actually verify
- You can't say what the sentence claims without rereading it

## When not to use this

- Legal, compliance, or license text with required wording
- Direct quotes, and prose the user wrote and asked you only to review
- Code, identifiers, log lines, and anything in a code block
- Creative writing where the voice is the deliverable

## Related

`humanizer` covers a different job: adding voice and opinion to essays and
blog posts, and it excludes technical documentation. `unslop` strips decoration
for clarity and applies to docs. Run `unslop` first, then `humanizer` if the
piece needs personality.
