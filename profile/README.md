<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/DYResearch/.github/raw/main/profile/assets/hero-dark.svg">
  <img alt="DY Research — know what is real. Independent technical intelligence: technical due diligence, worst-case timing, formal verification, a written verdict." src="https://github.com/DYResearch/.github/raw/main/profile/assets/hero-light.svg" width="100%">
</picture>

**[dyresearch.github.io](https://dyresearch.github.io)** · **[Engagements](https://dyresearch.github.io/#engagements)** · **[dy-wcet](https://github.com/DYResearch/dy-wcet)** · **[Try it live](https://dyresearch.github.io/wcet/)** · **[AxonOS](https://github.com/AxonOS-org)** · **[Radar](https://axonos-bci.github.io/axonos-community-radar/)**

[![CI](https://img.shields.io/github/actions/workflow/status/DYResearch/dy-wcet/ci.yml?branch=main&style=flat-square&label=dy-wcet%20CI&labelColor=0d1117)](https://github.com/DYResearch/dy-wcet/actions)
[![Kani](https://img.shields.io/badge/Kani-8%20proofs%20in%20CI-2ea043?style=flat-square&labelColor=0d1117)](https://github.com/DYResearch/dy-wcet#formal-verification)
[![Dependencies](https://img.shields.io/badge/dependencies-0-1f8fae?style=flat-square&labelColor=0d1117)](https://github.com/DYResearch/dy-wcet/blob/main/Cargo.toml)
[![Engagements](https://img.shields.io/badge/engagements-from%20%245%2C000-1f8fae?style=flat-square&labelColor=0d1117)](https://dyresearch.github.io/#engagements)
[![License](https://img.shields.io/badge/license-Apache--2.0%20OR%20MIT-8b949e?style=flat-square&labelColor=0d1117)](#licensing)

</div>

**DY Research is independent technical intelligence** for investors, founders and
engineering teams. Every claim a technology makes is traced to its code, its tests
and its evidence, and the engagement ends in a written verdict on what holds.

> Don't tell me the claim is correct. Give me the evidence: what is assumed, what
> is proven, what fails, and where the model stops being valid.

The tooling is open source. The verdict is yours.

---

## Engagements

| Engagement | The question it answers | Price |
|:--|:--|:--|
| **Snapshot** · 5 business days | What does this technology actually do, and what does its evidence support? | **$5,000** |
| **Focused Audit** · 2–3 weeks | Does one critical property — timing, determinism, concurrency — actually hold? | **$12,000** |
| **Due Diligence** · 3–4 weeks | Is the technology what the company says it is, and what could break the investment? | **$25,000** |

Fixed price and scope, confirmed in writing before any work begins; invoiced in
USD or EUR. Every engagement is carried out by the principal, start to finish.
**[Engagements and full scope →](https://dyresearch.github.io/#engagements)** ·
[the scope document](https://github.com/DYResearch/dy-wcet/blob/main/AUDIT.md) ·
[connect@axonos.org](mailto:connect@axonos.org)

---

## The toolchain

A response-time bound is only as good as what is fed to it and what it is used
for. The timing work is four tools, one per question, each doing its part in
integer arithmetic and refusing where it cannot justify an answer.

```text
dy-trace  →  dy-blocking  →  dy-wcet  →  dy-certify
execution    blocking        response    a certificate an
times        terms           times       auditor can re-run
```

| Tool | What it produces | Status |
|:--|:--|:--|
| [**dy-wcet**](https://github.com/DYResearch/dy-wcet) | Worst-case response times, exactly, with a named refusal wherever a bound cannot be justified | **Shipped** · 4.1.6 · 8 Kani proofs and 100 tests in CI · 0 dependencies |
| [dy-trace](https://github.com/DYResearch/dy-trace) | Execution-time inputs with their confidence stated | In design |
| [dy-blocking](https://github.com/DYResearch/dy-blocking) | Blocking terms derived from a resource graph, not estimated | In design |
| [dy-certify](https://github.com/DYResearch/dy-certify) | A signed schedulability certificate an auditor can re-run | In design |

<sub>A tool in design has a README that says what it will do and a section on what is not there yet. Nothing in design is claimed to work.</sub>

---

## In focus · dy-wcet

Two tasks. A runs 100 µs every 400 µs at higher priority; B runs 200 µs every
1,000 µs. The common answer for B is 400 µs. The correct one is **300 µs** — and
with other periods, the same mistake reports a deadline as met that is missed on
hardware.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/DYResearch/.github/raw/main/profile/assets/dywcet-schedule-dark.svg">
  <img alt="The schedule for the two tasks: A preempts B at the start, B runs from 100 to 300 microseconds and finishes long before its 1,000 microsecond deadline." src="https://github.com/DYResearch/.github/raw/main/profile/assets/dywcet-schedule-light.svg" width="100%">
</picture>

<sub>The schedule, drawn by simulating the scheduler rather than the formula. On 5,000 random task pairs it lands exactly on the analysis's answer.</sub>

**[Try it live →](https://dyresearch.github.io/wcet/)** · [Source](https://github.com/DYResearch/dy-wcet) · [The method](https://gist.github.com/AxonOS-BCI/3bef2ff217a3ece45e4958fac2c16c4a) · [The bounty](https://github.com/DYResearch/dy-wcet/blob/main/BOUNTY.md)

---

## One claim, traced

The method, applied to the practice's own tool: this is what every engagement
produces for every claim that matters.

| Step | For dy-wcet |
|:--|:--|
| **Claim** | Response times are computed exactly, in integer arithmetic |
| **Source** | Stated as a design rule in the [README](https://github.com/DYResearch/dy-wcet#why-it-exists) |
| **Code** | `u64` throughout, and no floating point — checked by [`audit.sh`](https://github.com/DYResearch/dy-wcet/blob/main/audit.sh) on every push |
| **Tests** | 100 tests, fifteen of them [derived by hand](https://github.com/DYResearch/dy-wcet/blob/main/tests/on_paper.rs) |
| **Proof** | 8 [Kani proofs](https://github.com/DYResearch/dy-wcet/tree/main/kani), each closing in CI, each bounded over the ranges it declares |
| **Verdict** | **Evidenced, within its stated scope** |

---

## Checked in public

| | |
|:--|:--|
| [**An RP2350 timer that stopped for minutes**](https://github.com/DYResearch/dy-wcet/blob/main/case-studies/embassy-6528.md) | An intermittent `embassy-time` failure traced through alarm arming and timer-queue liveness, with evidence and hypothesis kept apart. The standard of delivery for a Focused Audit |
| [**557 confident wrong answers**](https://github.com/DYResearch/dy-wcet/blob/main/CHANGELOG.md) | The same review, run on dy-wcet itself, found it had reported 557 task sets as meeting deadlines they miss. Found, fixed, and published in full |
| [**Two implementations, one answer**](https://github.com/DYResearch/dy-wcet/blob/main/docs/CROSSCHECK.md) | A second response-time analysis, written separately, agrees on a set where the fixed point is not the first value tried |

---

## Where the money goes

Revenue from every engagement funds [AxonOS](https://axonos.org) — an open-source,
deterministic systems layer for neurotechnology — and its path to independent
foundation governance. The fee pays for two things: the independent technical
intelligence you receive, and open infrastructure for the field.

**Funding AxonOS never shapes a verdict.** If a company under review competes with
AxonOS or builds on it, you are told at scoping, before you commit to anything.

---

## The ecosystem

<table>
<tr>
<td width="50%" valign="top">

**Build — [AxonOS](https://github.com/AxonOS-org)**<br>
The operating layer: kernel, signal pipeline, consent, protocol and SDK.
Specified openly, verified by machine.

</td>
<td width="50%" valign="top">

**Measure — [dy-wcet](https://github.com/DYResearch/dy-wcet)**<br>
Worst-case response-time analysis in integer arithmetic. Zero dependencies,
eight Kani proofs, every one closing in CI.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**Discover — [Radar](https://axonos-bci.github.io/axonos-community-radar/)**<br>
A living map of open neurotech: over a hundred projects, scored from public
evidence and refreshed every three hours.

</td>
<td width="50%" valign="top">

**Verify — [DY Research](https://dyresearch.github.io)**<br>
Independent technical due diligence for investors, founders and engineering
teams. A written verdict on what the evidence supports.

</td>
</tr>
</table>

<sub>The founder's account, with the live map and the demos: [AxonOS-BCI](https://github.com/AxonOS-BCI).</sub>

---

## Stated plainly

- **Not investment advice.** The findings are technical; the decision stays yours.
- **Not a certification.** dy-wcet is not a qualified tool under any safety standard, and no engagement issues or implies a standard's qualification.
- **Not a warranty.** What you receive is evidence and reasoning, set out so that it can be checked.

## Licensing

The tools are Apache-2.0 OR MIT, at your option. The engagements are written
agreements.

---

<div align="center">

© DY Research / Denis Yermakou

[dyresearch.github.io](https://dyresearch.github.io) · [connect@axonos.org](mailto:connect@axonos.org) · [LinkedIn](https://www.linkedin.com/in/axonos) · [AxonOS](https://axonos.org)

</div>
