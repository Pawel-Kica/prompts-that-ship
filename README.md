# prompts that ship

Prompts I actually run to ship faster with coding agents. No framework, no 40-step mega-prompt. Just the ones that earn their place.

Copy one, paste it into your agent (Claude Code, Cursor, whatever), fill in the `<bits>`.

## How to write a prompt that ships

No framework, just what works.

1. **Set the goal, not the steps.** Tell it what "done" looks like and let it find the path. "Clean this codebase until nothing's worth fixing" beats a 12-step checklist.
2. **Only the guardrails that matter.** "Zero behavior change, don't commit" earns its place. Everything else is noise.
3. **One job per prompt.** A small variation is a new prompt, not a bigger one.
4. **Tell it when to stop or ask.** The best prompts say what to do *and* when to come back to you.
5. **Write like you talk.** Ceremony does not make it smarter.

## The prompts

### ask-first

Stops the agent from guessing, and makes it show its plan before it touches anything.

```
Super important: if you don't fully understand what I want, ask me questions before you start. Do not guess.

Before you execute anything non-trivial, show me the plan first: the steps you're about to take. Wait for my OK, then go.

Keep it simple. Surface what's unclear, don't hide it.
```

### clean-the-codebase

Kills dead code and duplication without changing behavior. Runs on its own until there's nothing left to fix.

```
Clean this codebase. AFK mode: never ask, never stop to confirm, make every call yourself and keep going until nothing's worth fixing.

Scope: the whole repo. Read the repo's own conventions first (CLAUDE.md, style files) and follow them over your defaults.

Hunt, in priority order:
1. Duplication: same logic in 2+ places. Reuse an existing helper, or extract one near the call sites.
2. Dead code: unused imports, variables, functions, impossible branches, commented-out code.
3. Structure: deep nesting to early returns, god-functions split by concern.
4. Naming: misleading or abbreviated names, comments that just restate the code.

Rules:
- Zero behavior change. Every edit provably equivalent. If unsure, skip it.
- Reuse before abstracting. No new layer for a single call site.
- After each batch, run the repo's checks for the touched area. A failure means revert that edit and move on.
- Nothing destructive. Do not commit. Leave changes in the working tree.

End with a short summary: what changed by category, what checks you ran, anything you left alone on purpose.
```

### afk-mode

The opposite of ask-first. Hand it the wheel and walk away.

```
AFK mode. I'm away, take the task end to end.

Make every call yourself, never stop to confirm. When unsure, pick the sensible default and note it. Nothing destructive. Stop only when it's done or you're truly blocked.

When I'm back, leave a short report: what you did, and any calls worth checking.
```

### rubber-duck

Makes the agent help you debug by asking, not answering, so you find it yourself.

```
Be my rubber duck. I'm going to explain a problem I'm stuck on.

Don't solve it. Ask me simple questions, one at a time, the kind that make me explain my own reasoning out loud. Point out where my explanation skips a step or contradicts itself. Let me reach the answer myself.

Only if I'm still stuck after a few rounds, offer a direction.
```

*Why it works: [Rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging).*

### second-opinion

Turns the agent into a skeptic before you commit to a plan.

```
I'm about to do <thing>. Argue against it.

Give me the strongest case for a different approach, the one a smart skeptic would make. Steelman it, don't strawman. Then tell me which you'd actually pick, and why.

If my plan is genuinely fine, say so plainly. Don't invent objections.
```

## License

MIT
