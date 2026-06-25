# claude-structured-thinking

A Claude skill for structured thinking and decision-making: **拆 → 联 → 判**
(Decompose → Link → Judge). Drop it into your Claude skills folder and Claude stops
hand-waving — it walks you through a problem the way a sharp reviewer would, and
makes *you* do the thinking.

## What it does

When you face a fuzzy task, a design decision, or the question "is this actually
done?", `struct-thinking` runs you through three moves:

- **拆 / Decompose** — break the task into independent modules; for each, state what
  it *is* and what "done" looks like. Every module becomes an acceptance checkpoint.
- **联 / Link** — wire up the dependencies: what's a prerequisite for what, what runs
  in parallel, and which chain is the **critical path**.
- **判 / Judge** — the decision core, and it's *two* judgments:
  - **Ex-ante (decision)** — which path to take, using a reference frame + criteria
    *when you don't yet have data*.
  - **Ex-post (acceptance)** — is the built thing actually good, measured against the
    criteria you set during 拆 and 联.

The discipline it enforces: **set your acceptance criteria up front (判据前置)** — so
when the work is done you already hold a ruler, instead of asking "is this good?"
with nothing to measure against. And it never lets you forget that **交付验收 ≠
价值验收**: shipping every feature isn't the same as solving the need.

## When it triggers

Say `拆联判`, "help me think this through", "structure this", "help me decide", or
"is this actually done?" — or just describe a gnarly task and ask Claude to reason
through it with you.

## Install

**Option A — one click:** download the skill and click **Save skill**.

**Option B — manual:**

```bash
mkdir -p ~/.claude/skills
cp -r struct-thinking ~/.claude/skills/
```

Then start a Claude session and trigger it (see above).

## Example (before / after)

**Task:** "I want to add search to my app."

**Before** (plain Claude):
> Sure — you could use Algolia, Elasticsearch, or Postgres full-text search. Algolia
> is easiest, Elasticsearch is the most powerful, Postgres is cheapest. Want me to set
> one up?

**After** (`struct-thinking`):
> **拆** — (1) index pipeline *[done = docs ingested + reindex on change]* · (2) query
> API *[done = p95 < 200ms]* · (3) ranking *[done = top-3 relevance ≥ current baseline]*
> · (4) UI *[done = results render < 1s]*.
> **联** — pipeline is a prerequisite for the query API; ranking depends on the API;
> UI runs parallel to ranking. **Critical path = pipeline → API → ranking.**
> **判** — *Ex-ante:* no traffic data yet → start with Postgres FTS (borrowed
> reference: fine to ~1M rows), revisit only if p95 breaks. *Ex-post:* node = each
> "done" above; **value = % of searches that end in a click** — not just "search works".

The second answer is longer because it hands you a plan you can schedule *and* verify,
with the trade-off named and the success metric pinned before a line of code is written.

## License

[Apache-2.0](./LICENSE).
