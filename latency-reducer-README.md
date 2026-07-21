# latency-reducer

Always-active skill that reduces response time by 50%+ through parallelization and skipping non-essential steps, aiming to preserve full accuracy.

## What it does

Once active, it changes how Claude sequences and executes work — not what's technically correct — by:

- **Parallelizing independent operations** — multiple file reads, greps, globs, bash commands, or webfetch calls run together in one batch instead of one-by-one.
- **Skipping unnecessary steps** — writes code instead of explaining first, runs obvious commands without describing them, uses known paths instead of listing directories first, implements clear patterns without asking for confirmation.
- **Executing directly** — no "I'll now...", no describing the plan before doing it; action and narration are collapsed into one step.
- **Batching tool calls** — related tool calls go in a single message rather than spread across several.
- **Predictive loading** — when reading a file, also reads likely-needed neighbors (e.g. `auth.ts` → also `auth.test.ts`, `auth.types.ts`).
- **Skipping confirmation loops** — doesn't ask "Should I proceed?" or "Is this correct?" for routine, non-destructive, unambiguous work; verifies its own output instead.
- **Preferring fast search patterns** — tries `glob` before `grep` before full `read` before broader exploration.

## What it never skips

- Security checks
- Confirmation before destructive operations (delete, drop table, force push)
- Understanding codebase context for complex changes
- Reading a file before editing it
- Confirmation when the approach is genuinely ambiguous or the task scope is large

## When it does ask first

- Destructive actions
- Genuinely ambiguous requests with multiple valid approaches
- Large-scope tasks needing direction

## Persistence

Active on every response, every session, by default. Only two things turn it off:

- `stop latency-reducer`
- `normal mode`

## Intensity levels

| Level | Target speed gain | Accuracy |
|-------|-------------------|----------|
| `lite` | 30% faster | 100% |
| `full` (default) | 50%+ faster | 100% |
| `ultra` | 70%+ faster | 100% |

Switch with: `/latency-reducer lite|full|ultra`

## Execution flow

1. Parse intent — what does the user actually need?
2. Identify dependencies — what must come first?
3. Parallelize — what can run simultaneously?
4. Execute — do it, don't narrate.
5. Verify — only if non-trivial.
6. Output — result, not process.

## Interaction with other skills

- **cost-optimizer** — no conflict. cost-optimizer cuts words; latency-reducer cuts time.
- **ponytail** — no conflict. ponytail decides *what* to build; latency-reducer decides *how fast* to build it.
- **caveman** — no conflict. Both compress; caveman activating keeps latency-reducer's speed behavior intact.

## Anti-patterns it rejects

Phrases like "Let me think about this," "I'll start by...," "First, I need to...," "The approach I'll take...," and "Here's my plan..." are treated as narration to cut — the skill just does the thing instead of announcing it.

## Note on the benchmark claims

The "50%+ faster," "3x faster file reads," "4x faster research," and similar figures in `SKILL.md` are stated targets, not measured results — there's no accompanying test suite (unlike the cost-optimizer skill, which ships test files) to verify these numbers. Treat them as directional intent rather than benchmarked performance.
