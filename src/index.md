---
layout: default
---

# Introduction

**SDP** is a theorem disprover for separation logic entailments. It can
disprove an entailment F |- G by proving that the non-entailment F |/- G
is satisfable (that is, there exists a witness-model). For this purpose,
at its core, **SDP** is equipped with a proof system which contains
various inference rules designed to prove the existence of the
witness-model of non-entailments.

# Why need to disprove entailments?

During the verification of a program, a verifier often needs to handle
verification conditions appearing in the form of entailments. However,
proving separation logic entailments is known to be undecidable and most
of the existing techniques mainly focus on proving valid entailment.
Such proof is needed to confirm that a program satisfies its
specification.

However, these techniques often return an answer *unknown* when they
cannot prove an entailment. In this case, it is desired to examine
further if this *unknown* answer means the entailment is *invalid*. This
is important for the verifier to conclude whether a program violates its
specifications, i.e., there is a bug in the implementation of the
program. Also, when an entailment can be concluded invalid, its
counter-model can be used to generate failed test cases, which enable
programmers to understand and to fix the bug more effectively.

# How good SDP is?

We first experimented **SDP** with the task of disproving invalid
entailments collected from the separation logic competition
SL-COMP 2019. **SDP** can disprove 94.8% (182/192) of these entailments
with the average proving time of 1.38s per entailment. It outperformed
all state-of-the-art separation logic provers, which can prove at most
71.4% (137/192) of the benchmarks. The detailed results is presented
below.

|---------------------------------|-------|-------|---------|---------|---------|---------|
| Category                        | #Ents | Slide | Asterix | ComSPEN | Cyclist | SDP     |
|---------------------------------|-------|-------|---------|---------|---------|---------|
| singly linked-list              |   134 | 0     | 126     |     126 |      24 | **126** |
| nested linked-list              |    16 | x     | x       |       x |       0 | **16**  |
| skip list                       |    12 | x     | x       |       x |       0 | **10**  |
| doubly linked-list              |    18 | 0     | x       |       0 |      14 | **18**  |
| tree                            |     1 | 0     | x       |       x |       1 | **1**   |
|---------------------------------|-------|-------|---------|---------|---------|---------|
| singly linked-list (arithmetic) |     3 | x     | x       |       3 |       x | **3**   |
| doubly linked-list (arithmetic) |     8 | x     | x       |       8 |       x | **8**   |
|---------------------------------|-------|-------|---------|---------|---------|---------|
| **Total**                       |   192 | 0     | 126     |     137 |      39 | **182** |
|---------------------------------|-------|-------|---------|---------|---------|---------|


We also aim to evaluate the practical use of our disprover **SDP** on
disproving entailments generated from program verification. We first
equip **SDP** with the proving feature from the base tool **Songbird**
to create the enhanced prover **SDP+**. To examine a given entailment,
**SDP+** will first attempt to prove it. If **SDP+** fails after a
timeout (here, we set to 10s), then it will change to disprove the
entailment. Then, we integrate the new prover **SDP+** as the backend of
the verification system **Hip** and evaluate it on proving and
disproving entailments collected from the verification of buggy
programs written in a C-like language. 

In this experiment, the enhanced prover **SDP+** can prove 100% (93/93)
of the valid entailments and 99.2\% (117/118) of the invalid
entailments. This is because it inherits both the proving feature from
**Songbird** and the disproving feature of **SDP**. Note that **SDP+**
can efficiently prove all entailments in 0.23s per entailment. On the
other hand, also the disproving runtime of **SDP+** is a bit slower than
**SDP** (1.91s vs 0.16s per entailments), this overhead is still
acceptable since the difference of 1.75s (1.91 - 0.16) is only 17.5\% of
the timeout 10s which is set for the proving feature in **SDP+**.

Experiment with valid entailments

|--------------------|------|----------|-----|------|----------|-----|-------|
| Data Structures    | #Ent | Songbird | SDB | SDB+ | Songbird | SDB | SDB+  |
|--------------------|------|----------|-----|------|----------|-----|-------|
| singly linked-list |   11 |       11 | -   |   11 | 0.16s    | -   | 0.17s |
| doubly linked-list |   15 |       15 | -   |   15 | 0.17s    | -   | 0.18s |
| sorted linked-list |   38 |       38 | -   |   38 | 0.13s    | -   | 0.13s |
| tree               |    9 |        9 | -   |    9 | 0.88s    | -   | 0.88s |
| binary search tree |   11 |       11 | -   |   11 | 0.18s    | -   | 0.18s |
| avl tree           |    9 |        9 | -   |    9 | 0.21s    | -   | 0.22s |
|--------------------|------|----------|-----|------|----------|-----|-------|
| **Total**          |   93 |       93 | -   |   93 | 0.23s    | -   | 0.23s |
|--------------------|------|----------|-----|------|----------|-----|-------|

Experiment with invalid entailments

|--------------------|------|----------|-----|------|----------|-------|-------|
| Data Structures    | #Ent | Songbird | SDB | SDB+ | Songbird | SDB   | SDB+  |
|--------------------|------|----------|-----|------|----------|-------|-------|
| singly linked-list |   19 | -        |  19 |   19 | -        | 0.10s | 1.35s |
| doubly linked-list |   25 | -        |  25 |   25 | -        | 0.12s | 0.71s |
| sorted linked-list |   10 | -        |  10 |   10 | -        | 0.04s | 0.82s |
| tree               |   14 | -        |  14 |   14 | -        | 0.55s | 4.11s |
| binary search tree |   29 | -        |  28 |   28 | -        | 0.33s | 2.80s |
| avl tree           |   21 | -        |  21 |   21 | -        | 0.13s | 1.73s |
|--------------------|------|----------|-----|------|----------|-------|-------|
| **Total**          |  118 | -        | 118 |  118 | -        | 0.16s | 1.91s |
|--------------------|------|----------|-----|------|----------|-------|-------|

# Download

- Our prover SDP and the experimented entaiment benchmarks can be
  downloaded here: [SDB and its
  benchmarks]()

- The details of our experiment are reported in this PDF file:
  [experiment details]()
