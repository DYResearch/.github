<div align="center">

# DY Research

### Systems work with the arithmetic shown.

[![Rust](https://img.shields.io/badge/Rust-no__std-CE422B?style=flat-square&logo=rust&logoColor=white&labelColor=0e141d)](https://github.com/DYResearch)
[![Licence](https://img.shields.io/badge/Apache--2.0%20OR%20MIT-475569?style=flat-square&labelColor=0e141d)](#licensing)
[![Contact](https://img.shields.io/badge/connect%40axonos.org-0a4a8f?style=flat-square&labelColor=0e141d)](mailto:connect@axonos.org)

</div>

---

Real-time systems are full of numbers that nobody can reproduce. A worst-case
bound computed in floating point differs in its last bits between compilers. An
analysis that caps its iteration returns a number that looks like an answer. A
sum that wraps turns a system that misses deadlines into one that appears to
meet them.

None of these fail loudly. All of them fail in the direction that flatters the
result.

The work here is narrower than a platform and more specific than a library: the
pieces of real-time analysis where the arithmetic decides the answer, written so
that the arithmetic can be checked.

## What that means in practice

**Integers, everywhere the result matters.** Same input, same bits, any machine.
A property a floating-point implementation cannot offer and a test cannot pin.

**Refusal over approximation.** When a computation has no answer, it returns
that — not the last value before the loop gave up. An infinite response time
fails every deadline comparison it is put into, which is the safe direction.

**Checked arithmetic as a rule, not a habit.** Overflow is reported. A wrapped
sum in a schedulability test is the single worst arithmetic error available,
because it converts a real failure into an apparent success.

**No dependencies where none are needed.** A crate that computes a bound should
not pull in a runtime to do it.

---

<div align="center">

## Schedulability audit

**You send a task set. I return a response-time analysis, with every iteration
written out, and signed.**

For teams where a missed deadline is a machine acting late rather than a
dropped frame. The tool is open and free; what is paid for is someone reading
the set, naming the assumptions it rests on, and standing behind the result.

**[→ What it covers, what it does not, and a worked example](AUDIT.md)**

<sub>connect@axonos.org · payment in Dogecoin</sub>

</div>

---

## Repositories

| | What it is |
|:--|:--|
| [`dy-wcet`](https://github.com/DYResearch/dy-wcet) | Response-time analysis for fixed-priority task sets. Integer throughout; non-convergence and overflow both return an explicit unschedulable result. |

## What is not claimed

No hardware measurements are published here. These crates compute bounds from
execution times somebody else established, and a bound is only as good as its
inputs — if those come from a spreadsheet rather than an oscilloscope, the
result is arithmetic about a guess. The distinction is stated in each crate
rather than left to a reader.

No comparative benchmark against other real-time platforms has been performed.

## Who

Denis Yermakou. Also the author of [AxonOS](https://github.com/AxonOS-org), a
real-time operating layer for brain–computer interfaces, where these problems
arrived first and where the answers are used.

## Licensing

Apache-2.0 OR MIT for code, at your option. Every source file carries its SPDX
identifier and its copyright line.

---

<div align="center">

**DY Research** · [connect@axonos.org](mailto:connect@axonos.org) · [axonos.org](https://axonos.org)

© 2026 Denis Yermakou

</div>


