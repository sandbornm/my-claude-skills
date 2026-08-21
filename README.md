# my-claude-skills

Claude Code skills: static binary analysis (angr), dynamic binary instrumentation (Frida), network capture analysis (pcap), crash triage, and plain-language editing (unslop).

## Install

Install all plugins at once:

```bash
claude plugin add --from-marketplace sandbornm/my-claude-skills
```

Or install individual plugins:

```bash
# Static binary analysis (angr)
claude plugin add sandbornm/my-claude-skills/angr-analyzer

# Dynamic binary instrumentation (Frida)
claude plugin add sandbornm/my-claude-skills/frida-instrument

# Network capture analysis (pcap)
claude plugin add sandbornm/my-claude-skills/pcap-analyzer

# Crash triage and classification
claude plugin add sandbornm/my-claude-skills/crash-triage

# Plain-language editing
claude plugin add sandbornm/my-claude-skills/unslop
```

## Plugins

### angr-analyzer

Static binary analysis using angr's Python framework. Decompile executables, recover control flow graphs, find vulnerabilities via symbolic execution, and extract symbols from stripped binaries.

**Prerequisites:** Python 3.8+, `pip install angr`

### frida-instrument

Dynamic binary instrumentation using Frida. Trace function calls at runtime, hook APIs, inspect process memory, enumerate loaded modules, and perform instruction-level tracing on live processes.

**Prerequisites:** Python 3, `pip install frida-tools`

### pcap-analyzer

Network capture analysis for digital forensics and reverse engineering. Extract TCP/UDP streams, DNS queries, HTTP transactions, cleartext credentials, and transferred files. Detect C2 beaconing, port scanning, data exfiltration, and DNS tunneling.

**Prerequisites:** Python 3.8+, `pip install scapy` (optional: install tshark via Wireshark for enhanced features)

### crash-triage

Triage and classify program crashes by bug class and exploitability. Reproduce a crashing input under ASAN/UBSan or a debugger, classify ASAN reports and GDB/LLDB backtraces (memory-bug, segv, ubsan, assertion, trap, leak, …), and emit a severity-ranked verdict with the responsible source frame and a recommended action. Useful for reviewing fuzzer-found crashes and deciding what to report upstream.

**Prerequisites:** Python 3 (stdlib only). For reproduction: the target binary (ideally an ASAN+UBSan build); optionally `gdb` or `lldb` for a backtrace fallback.

### unslop

Plain-language editing for any prose a person will read — docs, READMEs, PR
descriptions, reports, chat replies. Removes eleven recurring tics of
machine-sounding writing: "it's not X, it's Y", borrowed-jargon modifiers
("load-bearing", "byte-identical", "surface area"), stacked nouns, SAT words,
throat-clearing, empty amplifiers, inflated significance, manufactured
profundity, rule-of-three lists, hedge stacks, and question mirroring. Ships a
substitution phrasebook and a grep detector. Keeps numbers, caveats, and genuine
terms of art, and forbids inventing specifics to replace vague ones.

Ships a `/unslop` slash command alongside the skill:

```
/unslop README.md docs/*.md     # rewrite in place, report each tic removed
/unslop --check CHANGELOG.md    # report only, change nothing
/unslop                         # rewrite the file you last edited
```

The skill also loads on its own whenever you're writing docs, PR descriptions,
or reports.

**Prerequisites:** none.

## Standalone skills

Not packaged as plugins. Copy the directory into `~/.claude/skills/` to use.

### tufte-data-viz

Data visualization in the tradition of Edward Tufte: maximize data-ink, erase
chartjunk, direct-label instead of using legends, small multiples.
