# SCOTUS Oral Argument Rhetoric Project — Scoping Notes

Captured from an initial scoping conversation on 2026-11-11. Nothing built yet — this is the pressure-test discussion before committing to a design.

## The idea

Analyze Supreme Court oral argument transcripts for rhetorical/argument patterns (hedging, interruption rate, analogy use, how advocates respond to justices' questions, etc.) and test whether these patterns correlate with which side wins the case. Data source: Oyez.org — public, speaker-labeled transcripts plus case outcomes.

## Why this project

Portfolio has an "Outside of the Terminal" section with a History & Philosophy angle. Goal is a project there with a real testable hypothesis and an actual finding — not just "look, I can call an embedding model." Alternatives considered and set aside:

- Philosophical-idea-lineage visualizer
- Embedding/clustering map of historical texts
- Motivational speech generator
- General fallacy/rhetoric detector for political speech

The SCOTUS version won out because the data is clean, public, outcome-labeled, and less politically fraught than analyzing politicians.

## Rough shape (not committed)

- Extract rhetorical/linguistic features per advocate per case (interruption rate, hedging language, analogy use, etc.)
- Train an interpretable model (logistic regression / gradient boosting) to predict winner
- Compare against a known legal-scholarship baseline (e.g. "side that got more justice questions tends to lose")
- Interactive demo: pick a case, see flagged patterns + prediction vs. actual outcome

## Pressure-test findings

**Hypothesis defensibility — partial, and this is the real risk.** There's existing legal-scholarship literature (Epstein/Landes/Posner on oral argument as information; Black/Treul/Johnson/Goldman on question-count effects) already showing that hostile-question volume toward a side predicts outcomes reasonably well. Rhetorical features (hedging, interruptions, question count) are largely correlated proxies for "justices think this side is losing" — they may add little once the baseline is controlled for. A null/marginal result on top of the baseline is a *likely* outcome, not an edge case, and should be treated as a legitimate, credible finding rather than a failure to avoid.

**Oyez data quality.** Good for structure — speaker-labeled transcripts, timestamps, docket outcomes, justice identity. Real gaps: vote margins/majority coalitions aren't always co-located with transcripts; "winner" is sometimes ambiguous (vacated/remanded, mixed rulings); sample size is small (~60-80 argued cases/term → a few terms gives ~300-500 cases, ~600-1000 advocate-side observations). Workable for a simple logistic regression; gradient boosting over a large feature set risks overfitting badly at this n. Keep the model deliberately simple.

**Scope guardrail.** Do NOT build a general fallacy detector — that's the "cute NLP demo" trap. Fallacy-tagging from transcripts without heavy manual annotation is unreliable (trusting an LLM's fallacy judgments as ground truth is circular). Scope down to more objectively extractable features: question count/rate per side, interruption rate, hedge-word frequency (lexicon-based), justice-advocate turn-taking patterns, maybe analogy/hypothetical density. Fallacy detection is at most a stretch goal, not a headline feature.

**Strongest framing to lead with:** replication-plus-extension. "Does the known justice-hostility effect hold up, and do advocate rhetorical patterns add anything on top of it?" This is testable, modest enough for a solo build, and has a built-in credibility anchor since the baseline isn't invented — it's a known result being tested against.

## Open decision

Whether the portfolio piece centers on the statistical finding (write-up + key charts) or on the interactive case-explorer demo — these pull engineering effort in different directions (analysis rigor vs. UI polish) and should be decided before further scoping.

## Next steps (not yet started)

- Decide: analysis-led vs. demo-led framing
- Scope exact feature list (question count/rate, interruption rate, hedge-word lexicon, turn-taking, analogy/hypothetical density)
- Check Oyez API/data access specifics (bulk download vs. scraping, rate limits, licensing)
- Decide number of terms/cases to pull given the small-n constraint
- Pick baseline replication target (which published question-hostility result to replicate first)
