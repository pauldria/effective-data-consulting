---
title: "Why metric definitions drift, and what actually stops it"
description: "Two dashboards disagree about active users. The cause is almost never the pipeline."
date: 2026-07-19
---

A familiar situation: two dashboards report active users, the numbers differ by
eleven percent, and the meeting stops while everyone tries to work out which is
right. Someone volunteers to reconcile them. Two weeks later a third dashboard
exists.

The instinct is to treat this as a data quality problem and go looking at the
pipeline. Occasionally that is where the answer is. Far more often both numbers
are computed correctly and they are answering different questions — one counts
any session, the other counts a meaningful action; one attributes to signup date,
the other to first activity; one excludes internal accounts and the other never
knew to.

Neither is wrong. There was simply never a decision about what the term means.

## Why it recurs

Metric definitions drift because defining them has no owner and no moment. A
dashboard gets built for a specific question under time pressure, and the
definition is whatever the person building it assumed that afternoon. That
assumption is then embedded in SQL, where it is invisible to everyone who later
reads the chart.

Multiply that across a few years and several analysts and you do not have one
definition of an active user. You have nine, and no record of which is intended
for what.

## What does not fix it

**A one-off reconciliation.** It resolves today's discrepancy and changes nothing
about the process that produced it. The next dashboard reintroduces the problem.

**A data dictionary nobody maintains.** A spreadsheet of definitions written
during a governance push, accurate for about a quarter, is worse than none — it
looks authoritative while being wrong.

**A tool purchase.** Semantic layers and metrics stores are useful *after* the
definitional decisions exist. They cannot make the decisions for you, and buying
one before the decisions are made simply relocates the disagreement.

## What does

**Name an owner per metric.** Not a team — a person, with the authority to
decide. Most drift is the absence of anyone empowered to settle the question.

**Define it once, in code.** The definition should live where the number is
computed, not in a document alongside it. Anything separable from the
computation will eventually diverge from it.

**Make definitions visible where numbers are read.** If a dashboard displays
"active users," a reader should be able to see the definition without asking.
Most disputes are resolved by making the assumption visible at the point of use.

**Change definitions deliberately.** Definitions should change — the business
changes. What matters is that changes are versioned and dated, so a chart that
shifts in March can be explained by a definition change rather than treated as an
incident.

## The part that is not technical

Underneath the tooling question is an organisational one: someone has to be
allowed to decide, and other people have to accept the decision. Where that
authority is absent, teams keep their own definitions because theirs is the one
they trust, and no amount of infrastructure will resolve it.

That is usually the actual finding. The pipeline is fine. The decision rights
were never assigned.
