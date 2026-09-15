# bus-4040-week-3-homework-skill
claude skill
# equity-research-report

A Claude skill that writes institutional-style equity research reports in the Davis SIMG
five-section format.

## What it is

A skill is a folder of instructions Claude loads when it recognizes a matching task. This one
encodes the house style of the A.D. & J.E. Davis Student Investment Management Group at the
University of Idaho — the report structure, the valuation method, and the sourcing standards
an investment committee expects — so that asking Claude for a write-up produces a report in
that format rather than a generic stock summary.

```
equity-research-report/
├── SKILL.md                  # Format, section budgets, citation rules, failure modes
└── references/
    ├── valuation.md          # Comp set construction, multiple derivation, DCF, WACC, sensitivity
    └── sourcing.md           # Source hierarchy, where each figure lives, citation formats
```

`SKILL.md` loads whenever the skill triggers. The reference files load only when Claude
reaches the step that needs them — valuation mechanics aren't in context while it's still
gathering figures.

## What it does

Given a ticker — or a filing, transcript, or spreadsheet you hand over — it produces a
report of roughly 1.5 pages:

**Header box.** Buy/Sell/Hold, price with as-of timestamp, fair market value, 1-year price
target, industry, P/E and EV/EBITDA (trailing and forward), market cap, 52-week range. Fair
value and price target are treated as separate claims; where they diverge, that gap becomes
an argument in Valuation.

**Five sections, in order:**

| Section | What it covers | Budget |
|---|---|---|
| Security Description | Business model, segment mix with figures, concrete competitive position | 150–200 w |
| Earnings Review | Last quarter vs. expectations, then the operating detail explaining it | 200–250 w |
| Valuation | Peer comps + light DCF, cross-checked, with a stated weighting | 250–300 w + tables |
| Forward Outlook | Next 4–8 quarters as checkable milestones, vs. management guidance | 200–250 w |
| Risks | 3–5 company-specific risks with magnitude, likelihood, and triggers | 150–200 w |

**Valuation is two approaches, reconciled.** Comps against a named 4–6 peer set plus the
stock's own historical multiple range, with the price target derived from an explicitly
justified multiple. Then a transparent five-year DCF — every assumption visible, WACC built
from its components rather than asserted — and a WACC-vs-terminal-growth sensitivity grid.
When the two disagree, the report says which it weights and why instead of quietly averaging.

**Every figure carries a source and period inline.** `revenue of $69.6B (+16% y/y) (FY26 Q4
10-K)`. The skill keeps three things visibly separate — reported fact, management guidance,
and consensus or author estimate — because conflating them is how a report quietly overstates
its confidence. Derived numbers cite their derivation. Unavailable figures get `n/a` and a
reason rather than an interpolation.

## How it was made

Built with Anthropic's `skill-creator` skill, which runs a draft → test → review → revise
loop.

1. **Specification.** Format, valuation method (multiples plus a light DCF), citation
   strictness (every number), and output format (Markdown) were settled before drafting.
2. **Draft.** `SKILL.md` plus the two reference files, written to explain *why* each
   convention exists rather than issue rules — the model follows reasoning it understands
   better than it follows imperatives.
3. **Test.** Three realistic prompts, each run twice: once with the skill, once without, as a
   baseline. The prompts were written the way an analyst actually types — `"write up NVDA for
   the committee, we vote thursday"`; a Costco request where the ask is to justify an
   expensive multiple rather than dismiss it; an Adobe request where the analyst arrives with
   a prior and wants it tested.
4. **Grade.** Ten objective assertions per run — header box completeness, section order,
   distinct fair value and target, named peers, DCF with sensitivity, stated weighting,
   inline citations, guidance attribution, company-specific risks, and word count measured by
   script rather than by eye.

**Result: 80% of assertions passed with the skill, 27% without.** The baselines produced
competent analysis in their own freeform structure — none of them produced the format.

Two assertions failed consistently and were fixed before release: reports ran 1,450–1,700
words against the 900–1,200 target, and derived figures (computed growth rates, backed-out
margins) were arriving uncited. `SKILL.md` now carries an explicit cut-list with a
count-before-delivering step, and a citation rule specifically covering derivations.

## How to use it

Ask for a report in whatever words come naturally. The skill is written to trigger on casual
phrasings, not just formal ones:

- `write up NVDA for the committee, we vote thursday`
- `do a report on Salesforce`
- `what's our thesis on this name`
- attach a 10-Q or a transcript and ask what to make of it

Hand over source documents when you have them — the skill treats user-supplied filings and
transcripts as outranking anything it finds on the web, and quotes management from a
transcript directly rather than through a news summary. Without documents it researches the
figures itself.

For multiple names, it writes each as a complete standalone report rather than merging them
into a comparison, since a committee votes on one name at a time.

## Where it helps

The mechanical parts of a write-up — assembling the header box, pulling current multiples,
building the comp table, running the DCF, formatting citations — take most of the hours and
produce none of the insight. This front-loads that work so the time goes to the thesis.

The citation discipline is the part that compounds. A report where every figure is traceable
survives committee scrutiny; one where a number can't be sourced loses the room even when the
analysis is right. The skill enforces that by default, including on the derived figures that
are easiest to leave bare.

It also makes reports gradeable after the fact. Forward Outlook is written as specific,
checkable milestones rather than themes, so a report can be revisited in six months and
scored against what actually happened — which is how coverage gets better over time.

## Extending it

The format lives in `SKILL.md` and the mechanics in `references/`. To adapt it for a
different group, change the header box fields and section headings in `SKILL.md`; to change
valuation convention — a different WACC approach, a different comp screen — edit
`references/valuation.md` without touching the rest.

Adding a new reference file means adding it to `references/` and pointing to it from
`SKILL.md` at the step where it's needed, with a line on when to read it.