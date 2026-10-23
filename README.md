# think-first

A skill for Claude Code, Codex and other coding agents that keeps you doing the thinking while the AI writes the code.

On any task with a real design decision, the agent:

1. **Frames** the work: for a large feature, it lists the decisions involved (at most five) and takes them one at a time.
2. **Seals** its own approach to a file in `~/.think-first/sealed/` *before* asking yours, so it cannot drift toward your answer.
3. **Asks** your approach in plain words. A rough one-liner is enough; "no idea" gets the concept explained, then the question again.
4. **Reveals** the sealed file and compares: where the two differ, which failure cases each handles, and a verdict: yours wins, mine wins, merge wins, or taste.
5. **Logs** the decision to `~/.think-first/decisions.md`.
6. **Implements** the accepted designs and reports what changed, what was tested, and anything it decided without you.

Mechanical work (obvious fixes, config, copy, established patterns) goes straight through. Saying "just do it" skips the loop for that task.

## Install

```sh
npx skills add kabeerx9/think-first
```

The [`skills`](https://github.com/vercel-labs/skills) CLI installs `skills/think-first` for Claude Code, Codex and other agents. You can also copy that folder by hand into `~/.claude/skills/` or `~/.agents/skills/`.

## Make it the default

An agent sees only the skill's description and decides whether to load it, and your own instructions outrank skills. If they tell the agent to act without asking, it will skip this one. Add one line to your global instructions (`~/.claude/CLAUDE.md` for Claude Code, `~/.codex/AGENTS.md` for Codex):

```markdown
- For any task with a real design decision, use the think-first skill before planning or coding; skip it only when I say "just do it".
```

To keep the log somewhere else, add the path to that line, for example `log decisions to ~/notes/decisions.md`.

## Why the sealed file

If the agent shows its plan first, you anchor on it and end up reviewing instead of thinking. If it reads your plan first, it tends to agree with you, or to call a small tweak of yours a merge. Writing its approach to disk before your answer exists, and showing that file unchanged afterwards, makes the comparison honest, including when your approach is the better one.

## License

[MIT](LICENSE)
