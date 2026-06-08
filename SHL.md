# The SHL Handbook
### Structured Human Language — talking to Claude on purpose

*A short, friendly guide. Read it once, write a few prompts, and it sticks.*

---

## 1. The one idea

Claude has no eyes, ears, or intuition about you. Whatever you send — text, an image, your voice, a file — gets turned into one long stream of **tokens** sitting in a window of memory. That stream is *all* Claude has to work with.

So good communication isn't about magic words. It's about giving Claude a **clean, well-organized stream** with no ambiguity about three things:

> **What do you want, what should Claude work from, and what should the answer look like?**

SHL is simply a habit of structuring your message so those three things are obvious. That's it. Everything below is detail.

---

## 2. Start with the Minimal SHL

You almost never need the full schema. Most good prompts are just three lines:

```
@goal    What outcome do I want, and why?
@task    What exactly should you do?
@output  What should the answer look like?
```

**Example — the lazy way (vague):**

> "Can you look at my function and tell me if it's okay?"

**Example — Minimal SHL:**

```
@goal    I want this function safe to ship to production.
@task    Review it for bugs, edge cases, and thread-safety.
@output  A short list: each issue, why it matters, and a fix. No intro.
```

Same request. The second one removes every guess Claude would otherwise have to make. That's the whole game.

---

## 3. The full schema (add fields only when you need them)

Think of these as optional dials. Reach for one when the Minimal SHL isn't enough.

| Field | What it's for | When to add it |
|---|---|---|
| `@goal` | The outcome and the *why* behind it | Always |
| `@task` | The concrete action to perform | Always |
| `@output` | The shape of the answer (format, length, tone) | Always |
| `@role` | A lens to think through ("act as a security reviewer") | When expertise or tone matters |
| `@context` | Stable background that doesn't change turn to turn | When the task depends on your situation |
| `@input` | The actual material to work on, fenced off | When you're handing over data, text, or code |
| `@constraints` | Hard rules and things to avoid | When there are real limits |
| `@examples` | One or two "input → ideal output" pairs | When format or style is hard to describe |
| `@think` | How much reasoning you want | When the task is tricky, or when you want speed |
| `@accept` | How Claude knows it succeeded | For high-stakes or easy-to-get-wrong tasks |

### A note on `@input` — the fence

When you paste data or code, **fence it** so Claude can't confuse your material with your instructions:

```
@input
<<<
public void process(Order o) {
    // ... the code you want reviewed
}
>>>
```

The `<<< >>>` markers say: *everything between these is raw material, not a command.* This single habit prevents a huge class of misunderstandings.

---

## 4. A worked example, start to finish

**A messy prompt:**

> "I have this old payment module nobody understands, can you help me figure out what it does and maybe write some docs, it's pretty important and there's no comments"

**The same thing in SHL:**

```
#shl v0.1

@goal
Understand an undocumented legacy payment module well enough to safely modify it.

@role
A senior engineer who specializes in reverse-engineering legacy systems.

@context
No documentation, no comments, no original authors available.
This module handles real money, so correctness matters more than speed.

@task
1. Explain what this code does in plain language.
2. Identify the main flow and any hidden side effects.
3. Flag anything risky or surprising.

@input
<<<
[paste the code here]
>>>

@output
- A 3-sentence summary first.
- Then a numbered walkthrough of the flow.
- Then a "Risks & Surprises" section.
Plain prose, no filler.

@think
Step by step — accuracy over speed.

@accept
I should be able to hand your summary to a new teammate and have them
understand the module without reading the code.
```

Notice what happened: nothing was added that you didn't already know. SHL just **pulled the intent out of your head and onto the page** where Claude can see it.

---

## 5. Seven gentle principles

1. **Separate the three things.** Instructions, material, and output-spec should never blur together. Fields and fences do this for you.
2. **Stable stays on top, changing stuff goes last.** Background that's always true (`@role`, `@context`) makes a great reusable header you paste every time.
3. **Big material first, your question last.** If you paste a long document, put it near the top and ask your question at the bottom. Claude pays closer attention that way.
4. **Describe the answer's shape before asking for it.** "Give me a table with three columns" saves you from re-asking.
5. **Show one example instead of describing the style.** A single "this is what good looks like" beats a paragraph of adjectives.
6. **Say what to avoid.** "Don't suggest rewriting it from scratch" is as useful as any positive instruction.
7. **Cut the noise.** Every irrelevant sentence competes for attention. Shorter and sharper wins. Contradictions are worse than vagueness — Claude will try to satisfy both and please neither.

---

## 6. Common beginner mistakes

- **Burying the ask.** The actual request is in sentence five of a paragraph. Put it under `@task`.
- **Pasting code with no fence.** Claude can't tell where your instructions end and your data begins. Use `<<< >>>`.
- **Asking for "good" output.** Good by whose measure? Use `@output` and `@accept` to define it.
- **Over-building.** Not every prompt needs ten fields. Start minimal; add a dial only when something's missing.
- **Stuffing everything into one giant prompt forever.** It's fine to go back and forth. SHL makes the *first* shot clean; conversation handles the rest.

---

## 7. Copy-paste template

```
#shl v0.1

@goal
(what outcome you want, and why)

@task
(the specific thing to do — number the steps if there are several)

@input
<<<
(any material, data, or code goes here)
>>>

@output
(what the answer should look like — format, length, tone)

@think
(quick | step-by-step | deep — only if it matters)
```

Delete the fields you don't need. Keep `@goal`, `@task`, `@output`.

---

## 8. Quick reference card

> **Minimal:** goal · task · output
> **Add as needed:** role · context · input · constraints · examples · think · accept
> **Golden rule:** remove every guess Claude would otherwise have to make.

---

*You don't have to master this. Just writing `@goal / @task / @output` for your next three prompts will already make a visible difference. The rest you'll grow into.*
