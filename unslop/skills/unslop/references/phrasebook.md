# Phrasebook

Lookup table for the `unslop` skill. Left column is the slop, right column is
what a person would say. Read every match in context before replacing — a few
of these are legitimate in the right field.

## Single-word swaps

| Slop | Plain |
|---|---|
| utilize, leverage (verb) | use |
| facilitate | help, let, make it possible to |
| delve into | look at, dig into, read |
| myriad | many |
| plethora | too many, lots of |
| commence | start |
| terminate | end, stop, kill |
| endeavor | try |
| ascertain | find out, check |
| requisite | needed |
| cognizant | aware |
| disparate | different, unrelated |
| salient | main, key |
| elucidate | explain |
| obviate | remove the need for |
| ameliorate | improve, fix |
| exacerbate | make worse |
| aforementioned | this, that, the one above |
| heretofore | until now |
| pursuant to | under, following |
| additionally, moreover, furthermore | also, and (or delete) |
| subsequently | then, later |
| initial, initially | first |
| numerous | many, six |
| sufficient | enough |
| demonstrate | show |
| indicate | show, mean |
| implement | build, write, add |
| methodology | method, how we did it |
| paradigm | model, way |
| landscape, ecosystem, tapestry | (delete, name the actual thing) |
| robust | (name the property: handles bad input, survives restarts) |
| seamless, frictionless | (delete, or say what steps are gone) |
| holistic, comprehensive | (delete, or list what's covered) |

## Phrase swaps

| Slop | Plain |
|---|---|
| in order to | to |
| prior to | before |
| subsequent to | after |
| in the event that | if |
| at this juncture, at this point in time | now |
| for the purpose of | for, to |
| with regard to, in terms of | about, for |
| due to the fact that | because |
| in spite of the fact that | although |
| a number of | some, three |
| the majority of | most |
| on a daily basis | daily, every day |
| is indicative of | shows |
| has the ability to, is able to | can |
| is capable of | can |
| make a determination | decide |
| perform an analysis of | analyze |
| provide assistance to | help |
| in close proximity to | near |
| it is recommended that you | you should, do X |

## Copula avoidance — all of these mean "is"

| Slop | Plain |
|---|---|
| serves as | is |
| functions as | is |
| acts as | is |
| operates as | is |
| represents | is |
| constitutes | is |
| stands as | is |
| exists as | is |

## Borrowed-jargon modifiers

Say the consequence. If the consequence is the same without the term, drop it.

| Slop | Plain |
|---|---|
| load-bearing | if this is wrong, X breaks |
| byte-identical | the same file, byte for byte (keep only in reproducible builds) |
| first-class | works directly, no plugin/wrapper needed |
| orthogonal | unrelated; change one without touching the other |
| surface area | how many entry points there are; how much can break |
| the long pole | the slowest part; the thing everything waits on |
| non-trivial | (give the size: two days, 400 lines, three services) |
| table stakes | the minimum; what everyone already has |
| step function | a jump; 3x, not 10% |
| impedance mismatch | the two models don't line up: X vs Y |
| hermetic | no network, no host state — same result every run |
| idempotent | running it twice does the same thing as once |
| the substrate, the primitive, the fabric | (name the actual component) |
| bandwidth (of a person) | time |
| north star | goal |
| unlock, unblock (as nouns) | (say what becomes possible) |
| step-change, inflection point | (give the number) |
| composable, pluggable | (say what plugs into what) |
| opinionated | picks defaults for you; hard to change |
| foot-gun | easy to misuse: doing X silently drops Y |

## Throat-clearing openers — delete

- It's important to note that
- It's worth mentioning that
- It should be noted that
- Notably, / Crucially, / Importantly,
- Fundamentally, / Essentially, / Ultimately,
- At its core,
- At the end of the day,
- When it comes to X,
- In today's fast-paced world,
- Let's dive in / Let's unpack this / Let's break it down
- I hope this helps! / Great question! / You're absolutely right!

## Empty amplifiers — replace with the number, or delete

significantly, substantially, dramatically, vastly, considerably, markedly,
incredibly, tremendously, truly, genuinely, remarkably, notably, quite simply,
absolutely, definitely, certainly, undoubtedly, arguably, veritable

## Antithesis constructions — cut the "not" half

- It's not X, it's Y
- This isn't about X — it's about Y
- Not just X, but Y
- X isn't the problem; Y is
- Less X, more Y
- The real question isn't X, it's Y
- X is only half the story
- That's not a bug, it's a feature

Keep the negation only when the text earlier asserted X and you are correcting it.

## Inflated significance — state the change, let the reader rank it

- a fundamental shift in how we think about X
- a paradigm shift
- redefines what it means to X
- stands as a testament to
- marks a turning point / a watershed moment
- changes everything about
- ushers in a new era of
- a game-changer
- revolutionizes / transforms (when nothing was measured)

Replace with the concrete change: what is different now, and by how much.

## Manufactured profundity — delete or fold into the previous sentence

- That's the whole trick.
- And that's the point.
- Simple as that. / That's it. / Full stop. / Period.
- Which is exactly the problem.
- Therein lies the rub.
- And that changes everything.
- The rest is details.

## Hedge stacks — one hedge maximum

may potentially, could possibly, might perhaps, somewhat unclear, generally
tends to, it seems likely that, in some cases it may be that, there is a
possibility that

Better than any hedge: name the gap. "I only tested this on Postgres 15."

## Chatbot artifacts — never ship these

- Certainly! / Of course! / Absolutely!
- As an AI / As a language model
- I don't have access to real-time data (say what you did check)
- Let me know if you'd like me to elaborate!
- In conclusion, / To summarize, (when the text is six lines long)
- Feel free to reach out with any questions

## Legitimate uses — do not "fix" these

| Term | Where it's real |
|---|---|
| leverage | finance: debt-to-equity |
| orthogonal | math, linear algebra, statistics |
| byte-identical | reproducible builds, artifact verification |
| idempotent | HTTP methods, REST design, retry semantics |
| hermetic | build systems (Bazel, Nix) |
| surface area | security: attack surface, when counting real entry points |
| non-trivial | math proofs, complexity arguments |
| primitive | cryptography: a cryptographic primitive |
| robust | statistics: a robust estimator |

## Known false positives in the grep detector

| Match | Why it fires wrongly |
|---|---|
| `functions as` | `functions` is often a plural noun: "export recovered functions as JSON" |
| `leverage` | finance and mechanics use it literally |
| `surface area` | geometry, and real attack-surface counting |
| `full stop` | British usage meaning the punctuation mark |
| `not just ... but` | fires on any nearby "but", including unrelated clauses |

Read every hit in context. The detector proposes; you decide.

Context decides. When the term has a definition the reader must know to act
correctly, keep it and define it once.
