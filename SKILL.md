---
name: prompt-optimizer
description: Rewrite a user's coding prompt with clearer, more precise word choices before sending to an agent. Replaces vague or ambiguous language with words LLMs interpret more reliably for code generation. Does not add intent or structure the user didn't ask for.
---

# Prompt Optimizer

Rewrites the user's prompt by swapping vague words for precise ones that produce better LLM code output. Preserves the user's original intent exactly — only the words change, not the meaning.

## Rules

1. **Never add intent.** If the user said "make a sandwich", don't add "with error handling and tests." They said sandwich.
2. **Never add structure.** Don't inject bullet points, numbered steps, or sections the user didn't ask for.
3. **Never expand scope.** If they said "small", keep it small. Don't elaborate on what "small" means.
4. **Only swap words.** Replace fuzzy terms with precise ones. The sentence shape stays the same.

## Word Swap Reference

These are common vague→precise swaps. Apply only when the vague word appears:

### Action Words
| Vague | Precise | Why |
|-------|---------|-----|
| make, do | build, create, implement | LLMs produce more complete code with specific construction verbs |
| fix | patch, correct, resolve | "fix" is ambiguous — could mean refactor, debug, or hotfix |
| change | modify, update, replace | specifies the nature of the change |
| output | generate, return, emit | clarifies whether it's stdout, a return value, or a side effect |
| handle | validate, catch, reject, route | "handle" hides what actually happens |
| use | call, import, invoke, apply | specifies the mechanism |
| add | append, insert, attach, register | specifies where/how |
| remove | delete, drop, unregister, strip | specifies the operation |
| check | assert, verify, test, compare | specifies what "checking" means |
| set up | configure, initialize, provision | specifies the lifecycle stage |
| get | fetch, read, query, extract, compute | "get" hides whether it's I/O, computation, or a cache hit |
| put | write, store, persist, assign | specifies the destination |
| run | execute, invoke, spawn, start | specifies the execution model |
| show | render, display, print, log | specifies the output target |

### Quality/Size Words
| Vague | Precise | Why |
|-------|---------|-----|
| good | correct, robust, readable | "good" means nothing to an LLM |
| nice | clean, idiomatic, well-structured | same problem |
| simple | minimal, single-purpose, no dependencies | "simple" can mean easy or minimal |
| small | compact, single-file, lightweight | specifies the constraint |
| fast | low-latency, O(n), cache-friendly | specifies what kind of fast |
| optimal | efficient, minimal-allocation, branch-free | "optimal" is meaningless without a metric |
| clean | idiomatic, lint-free, well-named | specifies what "clean" means |
| better | more readable, faster, smaller, safer | "better" needs a dimension |
| concise | terse, minimal-API, no boilerplate | specifies what to cut |
| proper | standard, conventional, spec-compliant | specifies whose standard |
| safe | validated, bounds-checked, sanitized | specifies against what |
| scalable | horizontally-scalable, O(log n), stateless | specifies the scaling dimension |
| modern | ES2024, current-stable, latest-LTS | pins to something concrete |

### Structural Words
| Vague | Precise | Why |
|-------|---------|-----|
| thing | value, entity, record, handler, struct | forces you to name what it is |
| stuff | fields, methods, config, dependencies | same |
| part | module, layer, stage, component | specifies the boundary |
| piece | function, block, segment, chunk | specifies granularity |
| way | pattern, strategy, approach, algorithm | specifies the abstraction level |
| like | such as, following the pattern of | removes ambiguity between "similar to" and "for example" |

## Process

1. Read the user's prompt
2. Identify vague words from the reference above
3. Swap each for the most accurate precise equivalent given context
4. Show the user the rewritten prompt
5. Ask: "Send this to an agent, or adjust?"
6. On approval, dispatch via the Agent tool

## Examples

**Raw**: "make me a small efficient sandwich"
**Optimized**: "build me a compact lightweight sandwich"

**Raw**: "get the data and show it nicely"
**Optimized**: "fetch the data and render it cleanly"

**Raw**: "fix the thing that handles user stuff"
**Optimized**: "patch the handler that validates user input"

**Raw**: "add a fast way to check if things are good"
**Optimized**: "insert a low-latency method to verify records are valid"

**Raw**: "set up a simple thing to run the jobs"
**Optimized**: "configure a minimal worker to execute the jobs"
