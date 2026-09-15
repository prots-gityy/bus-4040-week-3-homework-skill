---
name: equity-research-report
description: Writes institutional-style equity research reports in the Davis SIMG five-section format (Security Description, Earnings Review, Valuation, Forward Outlook, Risks) with a Buy/Sell/Hold header box, comps-plus-DCF valuation, and a source cited on every figure. Use this skill whenever the user asks for an equity research report, a stock write-up, a coverage note, an investment recommendation, a pitch on a ticker, an update on a name they cover, or a valuation of a public company — including casual phrasings like "write up NVDA for the committee", "do a report on Salesforce", "what's our thesis on this name", or when they hand over a 10-K, 10-Q, or earnings transcript and ask what to make of it. Also use it when revising or extending an existing report in this format.
---

# Equity research report (Davis SIMG format)

You are writing for an investment committee that will vote on a recommendation. They are
finance-literate, short on time, and skeptical. What persuades them is a chain of evidence
they can audit: a number, where it came from, and what it implies. What loses them is
confident prose with nothing underneath it.

Target length is about 1.5 pages per stock — roughly 900–1,200 words of body prose, not
counting tables or the header box. That constraint is the point, and it is the hardest part
of the format to hold: a well-researched draft naturally wants to run 1,600 words because
every paragraph is defensible on its own. Defensible is not the bar. Before delivering,
count the body words and cut to the target, because a committee reading eight names in an
hour will skim anything longer and you lose control of what they take away. The per-section
word budgets below add up to the target; treat them as real.

What to cut first, in order: background the committee already knows, sentences that restate
a table, hedging clauses that don't change the conclusion, and any second example where one
already made the point. What never gets cut: a figure, its source, or the reasoning that
links an assumption to the recommendation.

Output is Markdown unless the user asks for something else.

## Before writing: gather, then reconcile

Never draft from memory. Prices, multiples, guidance, and consensus all move, and a report
built on stale figures is worse than no report because it looks authoritative.

1. **Read what the user gave you first.** Filings, transcripts, or spreadsheets they supply
   are the primary source and outrank anything you find on the web. If they attached a
   transcript, quote management from it directly rather than paraphrasing a news summary.
2. **Fill the gaps by research.** Pull current price, market cap, 52-week range, trailing
   and forward multiples, the last reported quarter's actuals, and management's forward
   guidance. Sell-side consensus where you can find it. Check `references/sourcing.md` for
   where to look and which sources are reliable enough to cite.
3. **Reconcile conflicts out loud.** When two sources disagree on a figure, use the one
   closer to the company (filing > transcript > data aggregator > news article) and say so
   in a footnote if the gap is material.
4. **Note the as-of date.** Market data goes stale within the day. Stamp the header box with
   the date and time of the price you used.

If a figure you need simply isn't available, write `n/a` and say why in one clause. Do not
interpolate, do not average two guesses, and do not round a number into existence. An
investment committee that catches one manufactured figure stops trusting the whole report —
and rightly so.

## Citation discipline

Every number in the report carries its source and period inline, in parentheses, at first
use:

- `revenue of $69.6B (+16% y/y) (FY26 Q4 10-K)`
- `management guided FY27 revenue to $305–310B (Q4 FY26 earnings call, 8/27/26)`
- `consensus FY27 EPS of $13.42 (Visible Alpha, 9/9/26)`

The parenthetical can be shortened on repeat mentions of the same figure. Three distinctions
the committee cares about, and which the prose must keep visibly separate:

- **Fact** — reported, in a filing or press release. State it flatly.
- **Guidance** — what management said would happen. Attribute it to management every time:
  "management guided", "the company expects". Never let guidance drift into the narrative
  voice as if it were established.
- **Consensus / your own estimate** — what the Street or you expect. Label it as such, and
  when it is your own, show the assumption that produced it.

**Derived figures need sources too, and they are the ones that get missed.** A growth rate
you computed, a margin you backed out, a multiple you calculated from price and EPS, an
implied figure from a sensitivity grid — none of these came from a filing, so none of them
can carry a filing citation, and the temptation is to leave them bare. Bare is the failure.
Cite the inputs and the operation: `implied FY27E operating margin of ~24% (author
calculation from guided revenue and opex, Q4 FY26 call)`. The committee's check is whether
they can reproduce any number in the report from its citation; a derived figure with no
stated derivation fails that check as badly as an invented one.

Before delivering, sweep the body for bare numbers. Every figure should have a source, a
derivation, or an explicit `(author estimate)` with the assumption attached.

Avoid manufactured precision. If you triangulated a segment margin to somewhere in the low
30s, write "low-30s%", not "31.7%". False decimal places are the fastest way to look like
you did not do the work.

## The header box

Open with this table. It is the first and sometimes only thing a committee member reads, so
the recommendation and the three price figures have to be legible at a glance.

```markdown
| | |
|---|---|
| **Recommendation** | BUY / SELL / HOLD |
| **Price (as of 9/11/26)** | $XXX.XX |
| **Fair market value** | $XXX |
| **1-year price target** | $XXX |
| **Industry** | ... |
| **P/E (TTM / FY27E)** | XX.X / XX.X |
| **EV/EBITDA (TTM / FY27E)** | XX.X / XX.X |
| **Market cap** | $XXXB |
| **52-week range** | $XXX – $XXX |
```

Fair market value and the 1-year price target are different claims and should not be the
same number by accident. Fair value is what the business is worth on today's fundamentals;
the price target is where the stock trades in twelve months given expected multiple and
earnings. If they are far apart, that gap is itself an argument and belongs in Valuation.

## The five sections

Use these exact headings, in this order.

### Security Description

What the company does, how it makes money, and the two or three things that actually drive
the stock. Segment revenue mix with figures, not adjectives. Competitive position stated
concretely — share, switching costs, installed base — rather than as "leading player in a
growing market", which is true of every company ever written up and therefore tells the
committee nothing. Roughly 150–200 words.

### Earnings Review

The most recent quarter against expectations, then the why. Lead with the beat or miss on
revenue and EPS, then move immediately to the operating detail that explains it: which
segment drove it, what happened to margins, how the guide changed. Quote management where
their language is doing real work — a hedge, a first-time disclosure, a changed adjective on
a recurring metric. Committee members who follow the name will already know the headline
number; the value you add is the second layer. Roughly 200–250 words.

### Valuation

Show the work. Two approaches, cross-checked against each other:

1. **Multiples and peer comps** — the stock's current multiples against a named peer set and
   against its own three- or five-year range. Name the peers and say why they are comparable.
   Derive the target from a justified multiple: state the multiple you are applying, why that
   level (peer median, historical mean, a premium for a specific reason), and on what earnings
   base.
2. **A light DCF as a cross-check** — not a full three-statement model, but a transparent
   five-year projection with every assumption visible: revenue growth, operating margin,
   WACC, terminal growth. Show what the model spits out and, more usefully, how sensitive it
   is. A one-point move in terminal growth that swings fair value 30% tells the committee the
   DCF is not load-bearing, and saying so is more honest than presenting a point estimate.

`references/valuation.md` has the mechanics — how to build the comp set, how to pick a WACC
you can defend, and the sensitivity table format. Read it before doing the numbers.

When the two approaches disagree, do not split the difference silently. Say which one you
weight and why. Roughly 250–300 words plus tables.

### Forward Outlook

The next four to eight quarters. What has to be true for the recommendation to work, stated
as specific, checkable milestones rather than themes — "segment margin needs to hold above
X% through FY27" beats "continued execution". Anchor to management's guidance where it
exists and say where you are above or below it. This is the section that makes the report
worth revisiting in six months, so write it as something a future reader can grade you on.
Roughly 200–250 words.

### Risks

Three to five risks, each one specific to this company and each with a rough sense of
magnitude and likelihood. Generic risks — "macroeconomic conditions", "increased
competition" — are padding; a committee discounts a report that leads with them. The test
for each risk: could it be cut and pasted into a write-up of a different company? If yes,
sharpen it until it can't. Where a risk has an observable trigger, say what to watch. If one
of these risks would actually break the thesis, say which. Roughly 150–200 words.

## Common failure modes

- **Thesis-free reporting.** A report that describes the quarter accurately but never argues
  for the recommendation has not done its job. The committee is voting on a decision.
- **Sourcing the recommendation to no one.** If the Buy is driven by a multiple re-rating,
  say what re-rates it. If it's driven by earnings growth, show where the growth comes from.
- **Burying the disagreement.** Where your view differs from consensus, that difference is
  the most interesting thing in the report. Lead with it in Valuation or Forward Outlook,
  don't tuck it in a clause.
- **Padding to length.** 1.5 pages is a ceiling, not a quota. A tight one-pager that makes a
  clear argument beats two pages of hedged prose.

## Multiple stocks in one request

When asked to cover several names, write each as a complete standalone report with its own
header box and five sections, separated by a horizontal rule. Do not merge them into a
comparison piece unless asked — the committee reads these one vote at a time.
