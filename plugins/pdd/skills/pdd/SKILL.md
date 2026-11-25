---
name: pdd
description: Write or revise a Product Discovery Document (PDD), an opportunity assessment that decides whether an idea is worth a BRD at all. Use when someone has an idea, a request or a market signal and asks whether it is worth building, who it is for, how big it is, or why now; before a BRD, when nobody has yet shown the problem is real.
---

# Product Discovery Document

The PDD answers **whether**: is this problem real, for whom, how big, and is it worth pursuing now. It ends in a verdict, `Go`, `No-go` or `Parked`, and only a `Go` lets a business requirements document (BRD) build on it. It holds no solution beyond a sketch of the kind of answer, and no business case in detail: that is the BRD's job once the verdict is Go.

It is the first stage of a document chain, and optional: a BRD may still start from a source when the need is already settled, such as a contract, a regulation, or a decision taken elsewhere. A PDD is worth writing when the idea is still a hunch, when several ideas compete for one team, or when saying no needs a reason on record.

Before starting, read what the project already has: its document conventions (in `AGENTS.md`, `CLAUDE.md` or a docs guide, if any), its glossary, and any earlier documents under `docs/pdd/`, so numbering and vocabulary stay consistent. Where the project defines a document chain, folder layout or checker, follow it over the defaults here.

## Steps

1. **Gather the evidence.** Collect what the idea rests on: interview notes and their synthesis if the project keeps them (commonly under `docs/research/`), support tickets, analytics, a roadmap entry, a market report. Done when every claim you expect to make has a reference, or is marked as an assumption.
2. **Interview the user**, walking the ten questions below in order, one question at a time. Push for the evidence behind each answer, challenge vague ones ("everyone", "a lot", "soon"), and write "unknown" where there is none rather than an optimistic guess. Done when every question has an answer or an explicit "unknown" with who could find out.
3. **Draft** `docs/pdd/NNNN-<slug>.md`, numbered after the highest existing file, with the template below. Done when every `EV-n` names its reference and every `RISK-n` says how discovery would retire it.
4. **Decide.** Put the verdict to the user through the harness's question tool, with your recommendation first and its reason. The user decides; record their verdict in `Status`, and in `Verdict`, what would have to change to reverse it. Done when `Status` is `Go`, `No-go` or `Parked`, every reference in the document resolves, and the project's docs checker passes if it has one.
5. **Hand off.** On `Go`, the next step is a BRD that cites this PDD. On `Parked`, add an open entry to `TODO.md` at the repo root saying what would reopen it (`- [ ] <what would reopen it> #deferred (docs/pdd/NNNN-<slug>.md)`). On `No-go`, stop: the document is the record of why.

## The ten questions

After Marty Cagan's opportunity assessment (*Inspired*):

1. **Problem**: exactly what problem does this solve, and how painful is it?
2. **Customer**: for whom, specifically? "Everyone" is not an answer.
3. **Size**: how big is the opportunity: how many of them, and what is it worth?
4. **Alternatives**: what do they do today, and what would it cost them to switch?
5. **Advantage**: why are we the ones to solve it?
6. **Timing**: why now? What changed, or what closes if we wait?
7. **Reach**: how would it get to them?
8. **Success**: how will we know it worked: which number moves, by how much, by when?
9. **Critical factors**: what must be true for it to succeed: the riskiest assumptions.
10. **Verdict**: given all of the above, go, no-go or park?

## Template

```md
# PDD-NNNN: <Title>

**Status:** Draft | Go | No-go | Parked
**Owner:** <who is accountable for the verdict>
**Derived from:** <the evidence: a research file path, a URL, jira:KEY-123>

## Problem
<The problem in the customer's words, and how painful it is. Quote interviews where you can.>

## Customer
<Who, specifically: segment, role, situation.>

## Evidence
- EV-1: <What was observed, and its reference>

## Size
<How many, and what it is worth, with the arithmetic shown.>

## Alternatives
<What they do today, and the cost of switching.>

## Advantage
<Why us.>

## Timing
<Why now.>

## Reach
<How it gets to them.>

## Success
- OUT-1: <The outcome that would prove it worked: a number, a baseline, a date>

## Risks and assumptions
- RISK-1: <What must be true, how likely it is, and how discovery would test it>

## Verdict
<Go, No-go or Parked, and why. For Parked: what would reopen it. For every verdict: what would reverse it.>
```

## Quality bar

- Evidence over opinion: every `EV-n` names where it came from, and a problem nobody was observed to have is an assumption, filed under `RISK-n`.
- The problem is stated without the solution. "Dispatchers lose an hour a day re-keying jobs" belongs here; "build an integration" does not.
- Every `OUT-n` has a number and a date, so the BRD's success factors can refine it.
- A `No-go` is a result, not a failure: it saves the cost of every stage after it.

This skill copies no text. Its one-page assessment follows the one in `inspired-product` from [wondelai/skills](https://github.com/wondelai/skills) (MIT), whose verdicts of pursue, defer and decline become `Go`, `Parked` and `No-go` here.
