# The Template

```xml
<shl version="0.1">

<goal>
One or two lines: the outcome you want and *why* it matters.
The "why" lets Claude make good judgement calls when your instructions run out.
</goal>

<role>
The lens to think through, e.g. "A principal engineer specializing in
static analysis and compiler design." Optional — add when expertise or
tone should shape the answer.
</role>

<context>
Stable background that does not change between asks. Your environment,
constraints of the world, who the output is for. This is the block you
reuse as a header across many prompts.

- Use markdown bullets freely here.
- Keep it factual, not aspirational.
</context>

<task>
The concrete request. Number it when there are multiple steps.

1. First do X.
2. Then do Y.
3. Finally produce Z.
</task>

<input lang="" name="">
Fence raw material here — code, data, logs, a spec.
The `lang` and `name` attributes are optional hints.
If the content might contain `</input>`, rename the tag (e.g. <source_dump>).
</input>

<constraints>
Hard rules and things to avoid. Negative space bounds the search.

- Must: ...
- Must not: ...
</constraints>

<output>
The shape of the answer: structure, length, format, tone.

- Lead with a 3-sentence summary.
- Then a table / numbered list / code block as appropriate.
- No preamble, no filler.
</output>

<examples>
One or two input -> ideal-output pairs. Beats any adjective.

<example>
<in>a small representative input</in>
<out>the exact shape of output you want back</out>
</example>
</examples>

<think>
Reasoning budget: quick | step-by-step | deep.
Add only when it matters — depth for hard reasoning, "quick" to force brevity.
</think>

<accept>
Success criteria — how Claude knows it nailed it. Doubles as a self-check.

- A new engineer could act on this without asking follow-ups.
- No section contradicts another.
</accept>

</shl>
```
