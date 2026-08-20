<!--
SPDX-License-Identifier: CC-BY-SA-4.0
Copyright (c) 2026 Denis Yermakou <connect@axonos.org> — DY Research
-->

<div align="center">

# Schedulability audit

### You send a task set. I return a response-time analysis, signed.

[![Contact](https://img.shields.io/badge/connect%40axonos.org-5b8def?style=flat-square&labelColor=0e141d)](mailto:connect@axonos.org)
[![Payment](https://img.shields.io/badge/payment-Dogecoin-c2a633?style=flat-square&labelColor=0e141d)](#payment)
[![Tool](https://img.shields.io/badge/tool-dy--wcet%20(open)-3ecf8e?style=flat-square&labelColor=0e141d)](https://github.com/DYResearch/dy-wcet)

</div>

---

## What this is

Worst-case response-time analysis for a fixed-priority task set, with the
derivation shown line by line and a verdict per task.

It is aimed at teams building brain–computer interfaces, robotics, and anything
else where a missed deadline is not a dropped frame but a machine acting late.

**The tool is open.** [`dy-wcet`](https://github.com/DYResearch/dy-wcet) does the
arithmetic and you can run it yourself, today, for nothing. What is paid for is
the part a tool cannot supply: someone reading the set, saying which assumptions
it rests on, and signing the result.

## What this is not

**Not a measurement.** Execution times are your inputs. If they came from a
spreadsheet rather than an oscilloscope, this is arithmetic about a guess and
the report will say so.

**Not a code audit.** The analysis works on a model of your system. Whether the
code matches the model is a separate question, and a harder one.

**Not a regulatory certificate.** No accreditation stands behind this. It is
engineering evidence for your own reviewer, and any report claiming otherwise
would be lying about what it is.

**Not a safety opinion.** A schedulable task set can still be a dangerous
system. Timing is one property among many.

## What you send

A task set, in any legible format — JSON, CSV, or a table in an email.

| | Meaning |
|:--|:--|
| **C** | Worst-case execution time |
| **T** | Period, or minimum interarrival time for a sporadic task |
| **D** | Relative deadline. May be shorter than the period |
| **B** | Longest blocking by a lower-priority task holding a shared resource. Zero if nothing is shared |
| **priority** | The order, if it is not rate-monotonic |

Anonymise it if you want. The analysis does not need to know what the tasks do,
and a set of numbers with the names stripped is exactly as analysable.

## What you get

A report containing the utilisation, the response time of every task with each
iteration of the recurrence written out, the verdict per task, and the margin —
how much execution time can be added before the first deadline is missed.

Signed, with the tool version and a hash of the input, so the analysis and the
set it was run on cannot drift apart afterwards.

## Worked example

Three tasks from a plausible BCI gateway. Rate-monotonic priority, so the
shortest period preempts.

| Task | C | T | D | B |
|:--|--:|--:|--:|--:|
| Neural sampling | 100 µs | 1 000 µs | 1 000 µs | 0 |
| Motor command | 400 µs | 2 000 µs | 2 000 µs | 80 µs |
| Telemetry | 1 500 µs | 10 000 µs | 5 000 µs | 80 µs |

**Utilisation.** 100/1000 + 400/2000 + 1500/10000 = **0.45**. Necessary, not
sufficient: utilisation below one says a bound may exist, not that deadlines are
met.

**Neural sampling.** Nothing preempts it. R = 100 µs against a 1 000 µs
deadline. **Pass.**

**Motor command.** Starts at C + B = 480. At R = 480 one activation of sampling
fits, giving 580; at 580 still one fits, so 580 is the fixed point.

```
R = 480   preempted 1 × 100   →   580
R = 580   preempted 1 × 100   →   580   ← fixed point
```

R = 580 µs against 2 000 µs. **Pass.**

**Telemetry.** Starts at 1 580 and takes two iterations to settle.

```
R = 1580   preempted 2 × 100 + 1 × 400   →   2180
R = 2180   preempted 3 × 100 + 2 × 400   →   2680
R = 2680   preempted 3 × 100 + 2 × 400   →   2680   ← fixed point
```

R = 2 680 µs against 5 000 µs. **Pass.**

**Verdict.** Schedulable under the stated execution times.

> **The second iteration is where these go wrong.** Stopping at R = 2180 gives
> 2 180 µs, which is plausible, passes the deadline, and is *wrong* — at that
> response time sampling fits three times, not two. Here the error is
> conservative in the harmless direction. Move the deadline to 2 500 µs and the
> same mistake reports a pass on a set that fails.
>
> This is not hypothetical. It is the error in the first draft of this page,
> caught by running the numbers rather than reading them.

## Payment

Dogecoin. Price depends on the size of the set and how much of the input needs
untangling before it can be analysed at all — usually the larger part of the
work.

```
DMwHAhqVNWf7dyEznukxCufNS5rjuP5MTp
```

Send the set to **connect@axonos.org** first. If the analysis will not tell you
anything useful, I will say so and there is nothing to pay for.

## Why not just run the tool

You should. It is open, it costs nothing, and for debugging it is the right
answer.

Three things it will not do. It will not tell you that your blocking terms are
guesses, which they usually are. It will not notice that two of your tasks
share a resource nobody declared. And it cannot sign anything — a console log
is not an artefact a reviewer accepts, because nobody stands behind it.

---

<div align="center">

**DY Research** — Denis Yermakou

[github.com/DYResearch](https://github.com/DYResearch) · [connect@axonos.org](mailto:connect@axonos.org)

© 2026 Denis Yermakou

</div>

