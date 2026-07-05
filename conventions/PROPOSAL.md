# Proposals

Every change to how Oscille operates starts as a proposal, and every proposal has the same four parts. The shape exists so the evaluator can score the change later without asking its author anything (see [EVALUATOR.md](../EVALUATOR.md)).

In Linear, a proposal is an issue carrying the `proposal` label, with the four parts bold-led in the description. A friction that is real but has no stated change yet carries the `problem` label instead; it becomes a proposal when someone, human or operator, writes the other three parts.

## The four parts

- **Problem.** The friction as people feel it, not a guessed cause. "Planning eats an evening every week", not "we lack a planning tool".
- **Change.** What will exist that does not exist now. Concrete enough that a stranger could tell whether it was built.
- **Expected outcome.** The observable difference the change should make, stated before the work starts. This line is what the evaluator scores against, so a proposal that cannot state it is not ready.
- **How we know.** The single check that settles whether the expected outcome happened.

## Example

> **Problem.** Verdicts need evidence gathered from the week's ledger, and gathering it by hand costs the evaluator an hour before scoring can start.
>
> **Change.** A scheduled reporter that compiles the week's landed changes, expectations, outcomes, and costs into one packet, posted every Friday.
>
> **Expected outcome.** Scoring a week takes minutes, from one document, with no assembly.
>
> **How we know.** A Friday packet arrives unprompted and gets scored without a follow-up question.

## Rules

- One proposal is one change. A proposal that needs the word "and" in its Change line is usually two.
- The expected outcome is written before the work starts and is not edited after. If reality teaches you the expectation was wrong, say so in the verdict, not by rewriting history.
- A proposal the evaluator cannot score from its own text goes back to its author; that is the whole test of whether it is written well.
