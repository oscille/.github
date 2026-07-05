# The Evaluator

Version 0.1 · 2026-07-06 · provisional by design (see [CONSTITUTION.md](CONSTITUTION.md), Article 6)

This is the human-held method every landed change is scored against. The seed applies it to the companies it runs; it never applies it to itself. Whoever holds this document holds the one role that does not transfer.

## When scoring happens

Every Friday, the evaluator scores each change that landed that week, from the packet the operator compiles. Until the operator exists, the packet is assembled by hand. A change is scored once, within a week of landing, and the score is permanent; a later reversal is a new change with its own score.

## The three questions

Each change is scored on three questions. Each answer is `yes`, `partly`, or `no`, with one line of evidence a stranger could check.

1. **Did it solve the problem it claimed?** Judged by the felt outcome its proposal stated, never by effort spent or code shipped. If the proposal said "planning takes minutes, not an evening", the evidence is how long planning took.
2. **Did it move the handover?** The weekly count of human decisions that are not verdicts fell, or held while capability grew. A change that adds capability but also adds a new weekly human chore scores `no` here, whatever else it does.
3. **What did it cost?** Hours and money today; tokens and server time once the ledger meters them. Cost does not fail a change on its own; it is the denominator the first two answers are read against.

## Verdicts

A verdict is the three answers and their evidence, recorded as a comment on the change's issue, nothing more. Recording it removes the `verdict-pending` label. An unscored change stays `verdict-pending` and blocks nothing; the debt is visible, not paralysing.

## The one-way gate

A practice or component is promoted to `stable` only when question one and question two are both `yes` and the cost is justified in the verdict. Convenience, speed, or commercial success can nominate a candidate; they can never promote it. When the questions disagree, nothing is promoted; the disagreement is recorded and stands as a stop, not something to average away.

## Amendment

This method changes only by pull request to this file, stating the problem, the evidence the current method failed, the replacement, and the failure modes the replacement is expected to carry. Verdicts change every week because reality does; the method changes rarely, and never silently.
